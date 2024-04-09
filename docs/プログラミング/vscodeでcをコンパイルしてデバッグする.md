# vscodeでcをコンパイルしてデバッグする

**前提条件**

- WSlを使う。
- 拡張機能[cpptools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)をインストール済み。

``` title="フォルダ構成"
├ .vscode/
└ folder1/
  ├ program.c
  ├ a.out
  ├ program.o
  └ Makefile
```

``` makefile
# コンパイラはgccを使う
CC = gcc
# コンパイルオプション: -Wallは全ての警告を表示する, -std=c89はC89標準に準拠する,
# -pedantic-errorsは警告をエラーとして扱う, -Werrorは警告をエラーとして扱う,
# -gはデバッグ情報を付加する
CFLAGS = -Wall -std=c89 -pedantic-errors -Werror -g
TARGET = a.out
SRCS = $(shell find . -name "*.c")
OBJS = $(SRCS:.c=.o)

all: $(TARGET)

$(TARGET): $(OBJS)
	$(CC) $(CFLAGS) -o $(TARGET) $(OBJS)

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJS) $(TARGET)
```

``` json title="task.json"
{
  // See https://go.microsoft.com/fwlink/?LinkId=733558
  // for the documentation about the tasks.json format
  "version": "2.0.0",
  "tasks": [
    {
      "label": "build-c",
      "type": "shell",
      "command": "wsl -- make",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "options": {
        "cwd": "${fileDirname}"
      },
      "problemMatcher": ["$gcc"]
    }
  ]
}
```

``` json title="launch.json"
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "(gdb) Launch",
      "type": "cppdbg",
      "request": "launch",
      "program": "a.out",
      "args": [],
      "stopAtEntry": true,
      "cwd": ".",
      "environment": [],
      "externalConsole": false,
      "MIMode": "gdb",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ],
      "preLaunchTask": "build-c",
      "pipeTransport": {
        "pipeCwd": "${fileDirname}",
        "pipeProgram": "${env:windir}\\system32\\bash.exe",
        "pipeArgs": ["-c"],
        "debuggerPath": "/usr/bin/gdb"
      },
      "sourceFileMap": {
        "/mnt/c": "C:\\"
      }
    }
  ]
}
```