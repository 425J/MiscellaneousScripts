# Miscellaneous Scripts

## LabVIEW Run-Time Checker

Use this `*.bat` script to check if required LabVIEW Run-Time is installed.

```bat
@echo off

rem For the 64-bit version use C:\Program Files\National Instruments...
set "file_path=C:\Program Files (x86)\National Instruments\Shared\LabVIEW Run-Time\2021\lvrt.dll"
set "file_desc=LabVIEW 21.0.1f2 Run-Time Engine"

setlocal EnableDelayedExpansion

if exist "%file_path%" (

	for /f %%G in ('PowerShell -Command "&{(Get-ItemProperty -Path '%file_path%').VersionInfo.FileDescription -eq '%file_desc%'}"') do set "desc_match=%%G"
	if !desc_match! equ True (
		echo [92m"%file_desc%" installed[0m
	) else (
		echo [91m"%file_desc%" not installed[0m
		echo The file exists but its description does not match.
		echo            File path: "%file_path%"
		echo Expected description: "%file_desc%"
		echo.
	)

) else (
	echo [91m"%file_desc%" Not installed[0m
	echo The file is missing:
	echo Expected path: "%file_path%"
	echo.
)

pause
```

## TestStand Environment Launcher

Use this `*.bat` script to start TestStad Sequence Editor using a non-global environment.

```bat
@echo off

rem TestStad Sequence Editor path
rem    - For the 32-bit version use %TestStandBin% environment variable
rem    - For the 64-bit version use %TestStandBin64% environment variable
set TS=%TestStandBin%\SeqEdit.exe

rem TestStand Environment file path
set ENV=%~dp0Environment.tsenv

rem A sequence to be displayed when the editor is opened
set EP=%~dp0EntryPoint.seq

cd "%~dp0"
echo Start TestStand Sequence Editor ([93m%TS%[0m) using [93m%ENV%[0m environment file.
start "TestStand Sequence Editor" "%TS%" /env "%ENV%" "%EP%"
```
