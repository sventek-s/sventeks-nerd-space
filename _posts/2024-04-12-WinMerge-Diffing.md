---
published: true
---

# WinMerge Diffing Jar Files (Unzip, Decompile, then Diff)

> Credits to [@MCKSys Argentina a.k.a MCKSysAr](https://x.com/MCKSysAr/status/1682373126833799169) for this sorcery.

Follow the below steps:

1. Create a `Plugins.xml` file in the `%APPDATA%\WinMerge\MergePlugins` directory path, and copy the below contents to it.

```.xml
<?xml version="1.0"?>
<!--
Plugins.xml for WinMerge that will be read from
the %APPDATA%\WinMerge\MergePlugins path.

This allows us to avoid writing to the config file
in the C:\Program Files\WinMerge\MergePlugins\ path
that will be overwritten during an update installation.

Source: https://github.com/WinMerge/winmerge/discussions/948
-->
<plugins>
  <plugin name="DecompileClass">
    <event value="FILE_PACK_UNPACK" />
    <description value="Class decompile with FernFlower" />
    <is-automatic value="true" />
	<file-filters value="\.class$" />
	<unpacked-file-extension value=".java" />
    <extended-properties value="ProcessType=Decompilation;MenuCaption=Decompile Class file" />    
    <arguments value="" />
    <unpack-file>
      <command>"${WINMERGE_HOME}\Commands\class\unclass.bat" "${SRC_FILE}" "${DST_FILE}"</command>
    </unpack-file>
  </plugin>
</plugins>
```

2. Download fernflower decompiler from the following [source link](https://maven.neoforged.net/releases/net/minecraftforge/fernflower/).
If you have IntelliJ IDEA installed, you can use their embedded version of fernflower decompiler from the following path:
`C:\Program Files\JetBrains\IntelliJ IDEA 2024.2.2\plugins\java-decompiler\lib`.

3. Create a `unclass.bat` file in the `C:\ProgramFiles\WinMerge\Commands\class` folder path, and copy the below contents to it.
> Remember to change the decompiler tool path.

```.bat
@echo off
set SRC_FINAME=%~n1
set SRC_FILE=%1
set DST_FILE=%2
set DST_FOLDER=%~dp2
SET DST_FOLDER=%DST_FOLDER:~0,-1%
java.exe -jar "D:\tools\JavaDecompilers\fernflower-403.jar" %SRC_FILE% "%DST_FOLDER%"
copy /Y "%DST_FOLDER%\%SRC_FINAME%.java" %DST_FILE%
del "%DST_FOLDER%\%SRC_FINAME%.java"
```

<details>
<summary>4. Verify plugin is loaded from the plugins settings.</summary>
<br>

![image](https://github.com/user-attachments/assets/432c8a0a-95f2-4884-b07e-e58916a90f0a)

</details>

With the above done, you are set to go.

<details>
<summary>:heavy_plus_sign: You can also enable automatic unpacking to avoid multiple clicks.</summary>
<br>

![image](https://github.com/user-attachments/assets/850e97dd-b7e7-4602-85e9-a21717bb46fd)

</details>

