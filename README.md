# Issue description

It seems that MAUI incorrectly packs JAR dependencies into `.nupkg` (if adding with `AndroidMavenLibrary`).
The dependencies that provide AARs works fine. If adding the project locally from the folder, it also works fine.

# Steps to reproduce

- Add JAR library to Android Binding project (in the current example: `org.jetbrains.kotlinx:kotlinx-serialization-json-jvm`)
- Build `sdk-maui` project (`just -f sdk-maui/justfile build-android`)
- Create a release package (`just -f sdk-maui/justfile release`
- Add the package to `quickstart-maui` project (`just -f quickstart-maui/justfile add-local-release`)
- Run the `quickstart-maui` project (`just -f quickstart-maui/justfile run-android`)

Expected result: The app is working, white screen is displayed

Actual result: The app crashes with the following error:

```
java.lang.RuntimeException: Unable to get provider androidx.startup.InitializationProvider: androidx.startup.StartupException: java.lang.NoClassDefFoundError: Failed resolution of: Lkotlinx/serialization/json/Json;
```

If you uncomment 
`<AndroidMavenLibrary Include="org.jetbrains.kotlinx:kotlinx-serialization-json-jvm" Version="1.3.3" Bind="false" Condition="'$(TargetFramework)' == 'net9.0-android'" />` 
line in `quickstart-maui/QuickstartMaui.csproj` and build the project again, the app will work correctly.
