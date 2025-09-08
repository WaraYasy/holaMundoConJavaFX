# JavaFX Warnings Fix

This document explains the changes made to resolve the JavaFX warnings that appeared when running the project on Windows.

## Original Warnings

The following warnings were reported when cloning and running the project on Windows:

1. `[WARNING] Module name not found in . Module name will be assumed from module-info.java`
2. `WARNING: A restricted method in java.lang.System has been called`
3. `WARNING: java.lang.System::load has been called by com.sun.glass.utils.NativeLibLoader in module javafx.graphics`
4. `WARNING: Use --enable-native-access=javafx.graphics to avoid a warning for callers in this module`
5. `WARNING: Restricted methods will be blocked in a future release unless native access is enabled`

## Changes Made

### 1. Updated Java Version Configuration
- Changed Maven compiler source/target from Java 11 to Java 17
- Updated compiler plugin release version to 17
- This ensures consistency with the runtime environment

### 2. Updated JavaFX Maven Plugin
- Upgraded from version 0.0.6 to 0.0.8
- Added proper version property management
- Newer plugin version has better Java 17+ support

### 3. Fixed Module Name Detection
- Changed mainClass format from `org.dein.App` to `org.dein/org.dein.App`
- Added `runtimePathOption` set to `MODULEPATH`
- This resolves the "Module name not found" warning

### 4. Added Native Access Configuration
- Added `--enable-native-access=javafx.graphics` to commandlineArgs
- Added `--enable-native-access=javafx.graphics` to JVM options
- Added `--add-opens javafx.graphics/com.sun.glass.utils=ALL-UNNAMED`
- These resolve the native access and restricted method warnings

## Final pom.xml Configuration

The key changes in the `pom.xml` file:

```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <javafx.version>21.0.5</javafx.version>
    <javafx.maven.plugin.version>0.0.8</javafx.maven.plugin.version>
</properties>

<plugin>
    <groupId>org.openjfx</groupId>
    <artifactId>javafx-maven-plugin</artifactId>
    <version>${javafx.maven.plugin.version}</version>
    <executions>
        <execution>
            <id>default-cli</id>
            <configuration>
                <mainClass>org.dein/org.dein.App</mainClass>
                <runtimePathOption>MODULEPATH</runtimePathOption>
                <commandlineArgs>--enable-native-access=javafx.graphics</commandlineArgs>
                <options>
                    <option>--enable-native-access=javafx.graphics</option>
                    <option>--add-opens</option>
                    <option>javafx.graphics/com.sun.glass.utils=ALL-UNNAMED</option>
                </options>
            </configuration>
        </execution>
    </executions>
</plugin>
```

## Result

After these changes, the project should run on Windows without the reported warnings. The application will now:

- ✅ Properly detect the module name without warnings
- ✅ Handle native access correctly for JavaFX graphics
- ✅ Avoid restricted method access warnings
- ✅ Be prepared for future Java versions with stricter access controls

## Usage

To run the application after these changes:

```bash
mvn clean javafx:run
```

The application should start without the previously reported warnings.