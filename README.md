# Remodder

[![Rider](https://img.shields.io/jetbrains/plugin/v/24343.svg?label=Rider&colorB=0A7BBB&style=for-the-badge&logo=rider)](https://plugins.jetbrains.com/plugin/24343)
[![Patreon](https://img.shields.io/badge/Patreon-Support%20the%20project-ff5441?style=for-the-badge&logo=patreon)](https://www.patreon.com/zetrith)

WIP Rider plugin providing quality-of-life tools for modders of .NET games,
especially those using the [Harmony](https://github.com/pardeike/Harmony) runtime detour library.

## Features
- [Transpiler Preview](#transpiler-preview)

### Planned features
- Dropdowns for looking up and selecting reflection members in Harmony APIs (e.g. selecting the target of a `[HarmonyPatch]`)
- Deprioritizing publicized members in code completion suggestions
- I'm open for suggestions. Leave them in Issues if you have any

### Transpiler Preview
A tool for previewing the effects of Harmony transpilers right in the IDE.
The preview is a window showing the diff between the decompiled original method 
and the original after applying the transpiler under the mouse cursor.

Example transpiler:
```cs
[HarmonyPatch(typeof(ExampleClass), nameof(OriginalMethod))]
public static class ExampleClass
{
    public static int OriginalMethod(string s)
    {
        return 90 + s.Length;
    }

    public static IEnumerable<CodeInstruction> Transpiler(IEnumerable<CodeInstruction> insts)
    {
        yield return new CodeInstruction(OpCodes.Ldstr, "Hello World");
        yield return new CodeInstruction(
            OpCodes.Call, 
            AccessTools.Method(typeof(Console), nameof(Console.WriteLine), [typeof(string)])
        );
        
        foreach (var inst in insts)
        {
            if (inst.OperandIs(90))
                inst.operand = 42;

            yield return inst;
        }
    }
}
```

Resulting diff:

<img src="https://github.com/Zetrith/Remodder/blob/main/TranspilerPreviewScreenhot.png?raw=true" width="800"  alt="Transpiler Preview diff screenshot"/>

It works by querying the IDE for relevant referenced assemblies, 
temporarily loading them together with the project's compiled assembly 
and running the transpiler under the cursor in this context.

*User assemblies* is a list of assemblies to load which take precedence during transpiler execution and decompiling.
If you are compiling your project against reference assemblies (without method bodies),
you can use it to provide the original game assemblies to Remodder.

When you request a preview, the plugin executes the code of the project you are working on locally.
**Keep the possible security problems in mind when interacting with projects you don't trust.**

Current limitations:
- Only works on Harmony's class-with-attributes patches (like the one above)
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

## Support
You can support my work by becoming a Patron at https://www.patreon.com/zetrith
