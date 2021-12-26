# C++ Cheatsheet

## Index

* [VSCode](#vscode)
  * [Microsoft C++ Setup](#microsoft-c-setup)
  * [Clang Windows Setup](#clang-windows-setup)
  * [Links](#links)


## VSCode

### Microsoft C++ Setup

  * Follow: https://code.visualstudio.com/docs/cpp/config-msvc
  * Run VS Code outside the Developer Command Prompt (PowerShell version):
```json5
{
	"version": "2.0.0",
	"windows": {
		"options": {
			"shell": {
				"executable": "${env:ProgramFiles}/PowerShell/7/pwsh.exe",
				"args": [
					"-c \"&{Import-Module \"\"\"${env:ProgramFiles}/Microsoft Visual Studio/2022/Community/Common7/Tools/Microsoft.VisualStudio.DevShell.dll\"\"\"; pushd; Enter-VsDevShell 0c35fc9f; popd } &&"
				]
			}
		}
	},
	"tasks": [
		{
			"type": "shell",
			"label": "C/C++: cl.exe build active file",
			"command": "cl.exe",
			"args": [
				"/Zi",
				"/EHsc",
				"/nologo",
				"/Fe:",
				"${relativeFileDirname}\\${fileBasenameNoExtension}.exe",
				"${relativeFile}"
			],
			"options": {
				"cwd": "${workspaceFolder}"
			},
			"problemMatcher": [
				"$msCompile"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"detail": "compiler: cl.exe"
		}
	]
}
```

### Clang Windows Setup

  * Download [llvm-mingw](https://github.com/mstorsjo/llvm-mingw/releases) and extract to `$env:LOCALAPPDATA/Programs/llvm-mingw-ucrt-x86_64`
  * Run `Ctrl+Shift+P` 🡢 `C/C++: Edit Configurations (JSON)` 🡢 `c_cpp_properties.json`:
```json5
{
    "configurations": [
        {
            "name": "Win32",
            "includePath": [
                "${workspaceFolder}/**"
            ],
            "defines": [
                "_DEBUG",
                "UNICODE",
                "_UNICODE"
            ],
            "cStandard": "c17",
            "cppStandard": "c++17",
            "intelliSenseMode": "windows-clang-x64",
            "compilerPath": "${env:LOCALAPPDATA}/Programs/llvm-mingw-ucrt-x86_64/bin/clang++.exe"
        }
    ],
    "version": 4
}
```
  * Run `Terminal` 🡢 `Configure Default Build Task` 🡢 `C/C++: clang++.exe build active file` 🡢 `tasks.json`: 
```json5
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cppbuild",
			"label": "C/C++: clang++.exe build active file",
			"command": "clang++.exe",
			"args": [
				"-fdiagnostics-color=always",
				"-g",
				"${relativeFile}",
				"-o",
				"${relativeFileDirname}/${fileBasenameNoExtension}.exe"
			],
			"options": {
				"cwd": "${workspaceFolder}",
				"env": {
					"PATH": "${env:PATH};${env:LOCALAPPDATA}/Programs/llvm-mingw-ucrt-x86_64/bin"
				}
			},
			"problemMatcher": [
				"$gcc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"detail": "compiler: ${env:LOCALAPPDATA}/Programs/llvm-mingw-ucrt-x86_64/bin/clang++.exe"
		}
	]
}
```
  * Run `Run` 🡢 `Add Configuration...` 🡢 `C++ (GDB/LLDB)` 🡢 `clang++.exe - Build and debug active file` 🡢 `launch.json`:
```json5
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "clang++.exe - Build and debug active file",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [
                {
                    "name": "PATH",
                    "value": "${env:PATH};${env:LOCALAPPDATA}/Programs/llvm-mingw-ucrt-x86_64/bin"
                }
            ],
            "externalConsole": false,
            "MIMode": "lldb",
            "miDebuggerPath": "${env:LOCALAPPDATA}/Programs/llvm-mingw-ucrt-x86_64/bin/lldb-mi.exe",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "C/C++: clang++.exe build active file"
        }
    ]
}
```

### Links

  * [using Clang in windows 10 for C/C++](https://stackoverflow.com/q/63914108)
  * [Using GCC with MinGW](https://code.visualstudio.com/docs/cpp/config-mingw)
