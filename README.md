# Remodder

[![Rider](https://img.shields.io/jetbrains/plugin/v/RIDER_PLUGIN_ID.svg?label=Rider&colorB=0A7BBB&style=for-the-badge&logo=rider)](https://plugins.jetbrains.com/plugin/RIDER_PLUGIN_ID)

WIP Rider plugin providing quality-of-life tools for modders of .NET games,
especially those using the [Harmony](https://github.com/pardeike/Harmony) runtime detour library.

## Features
- Transpiler Preview

### Planned features
- Dropdowns for looking up and selecting reflection members in Harmony APIs (e.g. selecting the target of a `[HarmonyPatch]`)
- Deprioritizing publicized members in code completion suggestions
- I'm open for suggestions. Leave them in Issues if you have any

### Transpiler Preview
A tool for previewing the effects of Harmony transpilers right in the IDE.
The preview is a window showing the diff between the decompiled original method and the original with the transpiler applied.

It works by querying the IDE for relevant referenced assemblies, 
temporarily loading them together with the project's compiled assembly 
and running the transpiler under the cursor in this context.

When you request a preview, the plugin executes the code of the project you are working on locally.
**Keep the possible security problems in mind when interacting with projects you don't trust.**

Current limitations:
- Only works on Harmony's class-with-attributes patches (like the one in the screenshot above)
- Patches with `TargetMethods` are not supported

Good to know:
- Rebuild the project and refresh the preview to see changes in transpiler code
- The executed transpiler runs on the same runtime as the IDE (which is .NET 8 at the time of writing)

The feature is based on my past [TranspilerExplorer](https://github.com/Zetrith/TranspilerExplorer) project.

## Visual Studio support
I currently don't plan on making a VS version of Remodder but I'll consider it if
there's enough interest.

Rider is paid software but free licences for all of JetBrains' software
are available for [students](https://www.jetbrains.com/community/education/#students) and [open-source contributors](https://www.jetbrains.com/community/opensource/?var=1).