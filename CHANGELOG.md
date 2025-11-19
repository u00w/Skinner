# Changelog - Unity 6000.0.58f2 Upgrade

## Version Upgrade: 2017.1.0p3 → 6000.0.58f2

This is a major version upgrade spanning multiple Unity versions. The upgrade includes package system migration, API updates, and .NET Standard 2.1 migration.

---

## Changes Made

### 1. Unity Version Update
- **File**: `ProjectSettings/ProjectVersion.txt`
- **Change**: Updated `m_EditorVersion` from `2017.1.0p3` to `6000.0.58f2`
- **Impact**: Project now targets Unity 6000 (Unity 6)

### 2. Package Manager Integration
- **Files**: 
  - `Packages/manifest.json` (new)
  - `Packages/packages-lock.json` (new)
- **Change**: Created package manifest with Unity 6000-compatible packages
- **Packages Added**:
  - `com.unity.collab-proxy`: 2.5.2
  - `com.unity.feature.development`: 1.0.3
  - `com.unity.render-pipelines.universal`: 17.0.3 (URP)
  - `com.unity.textmeshpro`: 3.2.0-pre.12
  - `com.unity.timeline`: 1.8.7
  - `com.unity.ugui`: 2.0.0
  - `com.unity.visualscripting`: 1.9.4
  - All Unity built-in module packages

### 3. Project Settings Updates
- **File**: `ProjectSettings/ProjectSettings.asset`
- **Changes**:
  - Updated `serializedVersion` from 12 to 24 (Unity 6000 format)
  - Updated `scriptingRuntimeVersion` from 0 (Legacy .NET 3.5) to 1 (.NET Standard 2.1)
  - Updated `apiCompatibilityLevel` from 2 (.NET 2.0) to 6 (.NET Standard 2.1)
- **Impact**: Project now uses modern .NET runtime with access to .NET Standard 2.1 APIs

### 4. Assembly Definition Files (New)
- **Files**: 
  - `Assets/Skinner/Skinner.asmdef` (new)
  - `Assets/Skinner/Editor/Skinner.Editor.asmdef` (new)
- **Change**: Created assembly definition files for better compilation control
- **Benefits**:
  - Faster incremental compilation
  - Better organization of dependencies
  - Clearer separation between runtime and editor code
  - Required for Unity 6000 best practices

### 5. Code API Updates
- **Files**: 
  - `Assets/Skinner/SkinnerModel.cs`
  - `Assets/Skinner/Editor/SkinnerModelEditor.cs`
- **Change**: Updated bone weight API usage to suppress obsolete warnings
- **Details**: 
  - The `Mesh.boneWeights` property is deprecated in Unity 2019.1+ in favor of `GetAllBoneWeights()` and `SetBoneWeights()`
  - Added `#pragma warning` directives to suppress obsolete API warnings while maintaining compatibility
  - The legacy API still works in Unity 6000 but generates warnings
- **Note**: For full compatibility, consider migrating to the new bone weight API in a future update

---

## What Was Tested

### Automated Checks
- ✅ File structure validation
- ✅ Configuration file syntax
- ✅ Package manifest compatibility

### Not Tested (Requires Unity Editor)
- ❌ Asset database upgrade
- ❌ Shader compilation
- ❌ Material compatibility
- ❌ Scene loading
- ❌ Runtime behavior
- ❌ Build process

---

## Manual Steps Required

After merging this PR, the maintainer **MUST** perform the following steps:

### 1. Install Unity 6000.0.58f2
Download and install Unity Hub, then install Unity 6000.0.58f2 (6000.0.58f2)

### 2. Open Project in Unity 6000
```bash
# Open the project in Unity 6000.0.58f2
# Unity will automatically:
# - Upgrade the asset database
# - Reimport all assets
# - Recompile shaders
# - Update Library folder
```

**Expected Console Messages:**
- Asset import/reimport messages
- Shader compilation messages
- Possible deprecation warnings (these are expected and suppressed in code)

### 3. Verify Compilation
- Open Unity Editor
- Wait for all scripts to compile
- Check the Console for compilation errors
- Fix any errors that appear (likely none, but verify)

### 4. Test Core Functionality
- Open example scenes (if any exist)
- Test Skinner Model creation:
  1. Select a skinned mesh asset
  2. Right-click → Skinner → Convert Mesh
  3. Verify Skinner Model is created
- Test Skinner Source component
- Test Skinner renderers (Debug, Glitch, Particle, Trail)
- Verify rendering and visual effects work correctly

### 5. Run All Tests
```bash
# In Unity Editor:
# Window → General → Test Runner
# Run all Edit Mode tests
# Run all Play Mode tests
```

### 6. Build Test
- Attempt a build for your target platform(s)
- Test the built application
- Verify runtime performance

### 7. Shader Verification
- Check all Skinner shaders compile without errors
- Verify shader variants are generated
- Test shader replacement functionality
- Check compute shader compatibility

---

## Known Issues & TODOs

### Code Issues
- [ ] **TODO**: Migrate from deprecated `Mesh.boneWeights` API to `GetAllBoneWeights()`/`SetBoneWeights()` for cleaner code
  - Current implementation uses `#pragma warning disable` to suppress obsolete warnings
  - The legacy API still works but is not recommended for new code
  - Migration would require more extensive testing with actual skinned mesh assets

### Graphics Pipeline
- [ ] **TODO**: Evaluate URP (Universal Render Pipeline) compatibility
  - The project now includes URP 17.0.3
  - Current shaders may need conversion to URP shader graph or URP-compatible shaders
  - Test if current shader replacement technique works with URP

### Post Processing
- [ ] **TODO**: Review Post Processing Stack compatibility
  - The project includes a Post Processing folder
  - Post Processing Stack v2 has been replaced by Post Processing in URP
  - May need migration to URP post-processing or removal

### Platform Support
- [ ] **VERIFY**: Test on iOS (Metal) - was supported in 2017.1
- [ ] **VERIFY**: Test on macOS
- [ ] **VERIFY**: Test on Windows
- [ ] **VERIFY**: Android compatibility (was using min SDK 16, may need update)

### Settings Review
- [ ] **TODO**: Review and update Graphics Settings for Unity 6000
  - `GraphicsSettings.asset` still uses serializedVersion 7 (from Unity 2017)
  - May need updates for Unity 6000 graphics features
  - Consider enabling Graphics Jobs and GPU Skinning if not done

---

## API Compatibility Notes

### Breaking Changes in Unity 6000
The upgrade path from Unity 2017.1 to Unity 6000 includes many intermediate versions with breaking changes:

1. **Unity 2017 → 2018**: Nested prefab system, shader variants, particle system updates
2. **Unity 2018 → 2019**: Input System, new bone weight API, LWRP → URP
3. **Unity 2019 → 2020**: Asset Import Pipeline v2, rendering changes
4. **Unity 2020 → 2021**: Various API deprecations, performance improvements
5. **Unity 2021 → 2022**: Visual Scripting integration, addressables updates
6. **Unity 2022 → 6000**: Unity 6 rebranding, more rendering updates

### Currently Mitigated
- ✅ Bone weight API (using suppressed warnings)
- ✅ .NET Standard 2.1 compatibility
- ✅ Package manager integration

### May Require Manual Fix
- ⚠️ Shader compilation (needs Editor test)
- ⚠️ Render pipeline compatibility (may need URP conversion)
- ⚠️ Post-processing (may need replacement)
- ⚠️ Custom editor scripts (may have API changes)

---

## Rollback Instructions

If the upgrade causes critical issues:

1. **Checkout previous version**:
   ```bash
   git checkout <previous-commit-hash>
   ```

2. **Delete Library folder** (contains cached Unity data):
   ```bash
   rm -rf Library/
   ```

3. **Open in Unity 2017.1.0p3**

---

## Additional Resources

- [Unity 6 Release Notes](https://unity.com/releases/editor/whats-new/6000.0.0)
- [Unity Upgrade Guide](https://docs.unity3d.com/Manual/UpgradeGuides.html)
- [API Compatibility](https://docs.unity3d.com/6000.0/Documentation/Manual/dotnetProfileSupport.html)
- [Package Manager Documentation](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@latest)

---

## Summary

This upgrade establishes the foundation for Unity 6000 compatibility but requires extensive testing in the Unity Editor to complete. The changes made are minimal and focused on configuration updates, with code changes limited to suppressing deprecation warnings. A full production deployment should include thorough testing of all Skinner features, shader compilation, and runtime behavior across all target platforms.

**Recommendation**: Test thoroughly in a development environment before deploying to production.
