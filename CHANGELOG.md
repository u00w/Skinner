# Changelog

## Unity 6 Upgrade (In Progress)

### Overview
This release upgrades the Skinner project from Unity 2017.1.0p3 to Unity 6.0.0f1. This is a major version upgrade spanning several Unity versions and requires careful attention to API changes and deprecations.

### Changes Made

#### Project Configuration
- **ProjectVersion.txt**: Updated `m_EditorVersion` from `2017.1.0p3` to `6.0.0f1`
- **Package Manager**: Created `Packages/manifest.json` with Unity 6-compatible packages:
  - Universal Render Pipeline (URP) 17.0.3 (for future migration from legacy post-processing)
  - Text Mesh Pro 4.0.0
  - Timeline 1.8.6
  - Unity UI (uGUI) 2.0.0
  - Test Framework 1.4.5
  - IDE integrations (Rider, Visual Studio, VS Code)
  - All standard Unity module packages

#### Code Updates - API Compatibility

##### VR/XR API Changes
- **File**: `Assets/Skinner/SkinnerSource.cs`
- **Change**: Replaced deprecated `UnityEngine.VR.VRSettings` with `UnityEngine.XR.XRSettings`
- **Reason**: The VR namespace was deprecated in Unity 2017.2+ and replaced with XR namespace
- **Impact**: Minimal - XR API is backward compatible

##### Mesh Bone Weights API
- **Files**: 
  - `Assets/Skinner/SkinnerModel.cs`
  - `Assets/Skinner/Editor/SkinnerModelEditor.cs`
- **Change**: Added `#pragma warning disable 0618` / `#pragma warning restore 0618` around `Mesh.boneWeights` usage
- **Reason**: `Mesh.boneWeights` was deprecated in Unity 2019.1+ in favor of `GetAllBoneWeights()` and `SetBoneWeights()`
- **Approach**: Used pragma directives to suppress obsolete warnings while maintaining compatibility. The old API still works but generates warnings.
- **Future Work**: For full Unity 6 best practices, consider migrating to the new bone weights API in a future update

### Testing Status

#### ✅ Completed
- [x] Updated project version files
- [x] Created package manifest with Unity 6 packages
- [x] Fixed deprecated VR API (UnityEngine.VR → UnityEngine.XR)
- [x] Addressed bone weights API obsolete warnings
- [x] Documented all changes

#### ⚠️ Requires Unity Editor (Cannot be automated)
The following items require the Unity Editor to be installed and cannot be completed programmatically:

- [ ] **Editor Compilation**: Open the project in Unity 6 Editor to verify all scripts compile without errors
- [ ] **Shader Compatibility**: Verify all shaders (particularly replacement shaders) work correctly in Unity 6
- [ ] **Runtime Testing**: Test all Skinner components (SkinnerSource, SkinnerParticle, SkinnerTrail, SkinnerGlitch, SkinnerDebug) in play mode
- [ ] **Post-Processing Migration**: Evaluate migrating from legacy Post-Processing stack to URP post-processing
- [ ] **Graphics API Validation**: Test on target platforms (Windows, macOS, iOS) with Unity 6 graphics backends
- [ ] **Performance Validation**: Ensure GPU skinning and render performance is maintained

### Known Issues & Recommendations

#### Legacy Post-Processing Stack
The project currently includes the legacy Post-Processing stack (in `Assets/PostProcessing/`). This is deprecated in favor of:
- **URP Post-processing** (if using Universal Render Pipeline)
- **HDRP Post-processing** (if using High Definition Render Pipeline)

**Recommendation**: The URP package has been added to the manifest. Consider migrating to URP for better Unity 6 support and performance.

#### Shader Replacement System
The project uses `Camera.RenderWithShader()` for vertex baking. Verify this still works correctly in Unity 6 as shader replacement has had some changes over the versions.

#### Assembly Definition Files
The project does not currently use assembly definition (.asmdef) files. While not required, adding them can:
- Improve editor compilation times
- Better organize code dependencies
- Enable better compatibility with Package Manager workflows

**Recommendation**: Consider adding .asmdef files in a future update if compilation times become an issue.

### Manual Steps for Maintainers

After merging this PR, maintainers must perform the following steps:

1. **Open in Unity 6**: Open the project in Unity 6.0.0f1 or later
   - Unity will show a project upgrade dialog - proceed with the upgrade
   - Unity may regenerate meta files and Library folder

2. **Resolve Any Compilation Errors**: 
   - Check the Console for compilation errors
   - Most should be resolved, but platform-specific code may need attention
   - Check for shader compilation errors

3. **Test Core Functionality**:
   ```
   a. Open a test scene with a character
   b. Convert a mesh to Skinner Model (Assets → Skinner → Convert Mesh)
   c. Attach SkinnerSource to the character's SkinnedMeshRenderer
   d. Create renderer objects (SkinnerDebug, SkinnerParticle, etc.)
   e. Enter play mode and verify effects render correctly
   ```

4. **Regenerate IDE Project Files**: 
   - For Visual Studio: Assets → Open C# Project
   - For Rider/VS Code: Regenerate via IDE integration settings

5. **Re-import Assets** (if needed):
   - If textures, materials, or prefabs appear broken, try:
     - Assets → Reimport All

6. **Platform Testing**:
   - Test builds on target platforms (Windows, macOS, iOS)
   - Verify graphics backend compatibility (Metal, DirectX, Vulkan)

7. **Optional - Post-Processing Migration**:
   - If visual quality is important, consider migrating to URP
   - Create a URP Asset and assign it in Project Settings → Graphics
   - Migrate legacy post-processing effects to URP equivalents

### Breaking Changes
- Requires Unity 6.0.0f1 or later
- May require shader updates if using custom shaders with the project
- Legacy VR support code now uses XR APIs (functionally equivalent)

### Compatibility Notes
- **Unity Version**: 6.0.0f1+
- **Platforms**: Windows, macOS, iOS (as per original README)
- **Graphics APIs**: DirectX 11/12, Metal, Vulkan (GLES3 and WebGL untested)
- **Render Pipelines**: Built-in RP (current), URP (package added for future migration)

### References
- [Unity 6 Release Notes](https://unity.com/releases/unity-6)
- [Unity API Upgrade Guide](https://docs.unity3d.com/6000.0/Documentation/Manual/UpgradeGuides.html)
- [XR API Migration](https://docs.unity3d.com/Manual/xr_input.html)
- [Mesh Bone Weights API](https://docs.unity3d.com/ScriptReference/Mesh.GetAllBoneWeights.html)
