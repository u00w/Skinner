# Changelog

## [Unity 6000.0.58f2 Upgrade] - 2025-11-19

### Overview
This release upgrades the Skinner project from Unity 2017.1.0p3 to Unity 6000.0.58f2. This is a major version upgrade spanning multiple Unity generations.

### Original Configuration (Backup)
- **Unity Editor Version**: 2017.1.0p3
- **API Compatibility Level**: .NET 2.0 (value: 2)
- **Scripting Runtime Version**: Legacy .NET 3.5
- **Package Manager**: Not present (pre-Package Manager project)
- **Assembly Definition Files**: None (implicit assembly compilation)

### Changes Made

#### 1. Unity Version Update
- **File**: `ProjectSettings/ProjectVersion.txt`
- **Change**: Updated `m_EditorVersion` from `2017.1.0p3` to `6000.0.58f2`

#### 2. Package Manager Integration
- **File**: `Packages/manifest.json` (NEW)
- **Change**: Created manifest.json with Unity 6000-compatible package versions
- **Packages Added**:
  - `com.unity.collab-proxy`: 2.5.2 (Unity Version Control)
  - `com.unity.feature.development`: 1.0.3 (Development tools bundle)
  - `com.unity.ide.rider`: 3.0.31 (JetBrains Rider support)
  - `com.unity.ide.visualstudio`: 2.0.22 (Visual Studio support)
  - `com.unity.ide.vscode`: 1.2.5 (VS Code support)
  - `com.unity.render-pipelines.universal`: 17.0.3 (URP for Unity 6000)
  - `com.unity.test-framework`: 1.4.5 (Testing framework)
  - `com.unity.textmeshpro`: 3.2.0-pre.10 (TextMesh Pro)
  - `com.unity.timeline`: 1.8.7 (Timeline)
  - `com.unity.ugui`: 2.0.0 (Unity UI)
  - All Unity built-in modules (1.0.0)

#### 3. API Compatibility Level Update
- **File**: `ProjectSettings/ProjectSettings.asset`
- **Change**: Updated `apiCompatibilityLevel` from `2` (.NET 2.0) to `6` (.NET Standard 2.1)
- **Reason**: Unity 6000 requires .NET Standard 2.1 for modern C# features and better performance

#### 4. Deprecated API Updates
- **File**: `Assets/Skinner/SkinnerSource.cs`
  - **Line 191-194**: Updated `UnityEngine.VR.VRSettings` to `UnityEngine.XR.XRSettings`
  - **Reason**: VR namespace was deprecated and moved to XR in Unity 2019+
  - **TODO Added**: Verify XR.XRSettings compatibility with modern XR systems

- **File**: `Assets/Skinner/SkinnerModel.cs`
  - **Lines 43, 87**: Added `#pragma warning disable CS0618` for deprecated `boneWeights` API usage
  - **TODO Added**: Replace with `GetAllBoneWeights()` and `SetBoneWeights()` in future update
  - **Reason**: Direct array access to boneWeights is deprecated; suppressed for now to maintain functionality

- **File**: `Assets/Skinner/Editor/SkinnerModelEditor.cs`
  - **Line 33**: Added `#pragma warning disable CS0618` for deprecated `boneWeights` API usage
  - **TODO Added**: Replace with `GetAllBoneWeights()` in future update

### What Was NOT Changed
- **Assembly Definition Files**: Not added (none existed in original project)
- **Shader Code**: Not modified (requires Unity Editor for testing)
- **Material Assets**: Not modified (requires Unity Editor)
- **Scene Files**: Not modified (requires Unity Editor)

### Testing Performed
- Static file validation: All configuration files are valid YAML/JSON
- C# compilation check: NOT performed (requires Unity Editor)
- Runtime testing: NOT performed (requires Unity Editor)

### Known TODOs and Limitations

#### High Priority (Breaking API Changes)
1. **BoneWeights API**: The deprecated `mesh.boneWeights` property usage is suppressed with `#pragma warning disable CS0618`. This should be replaced with:
   - `mesh.GetAllBoneWeights()` and `mesh.GetBonesPerVertex()` for reading
   - `mesh.SetBoneWeights()` for writing
   - Affects: `SkinnerModel.cs`, `SkinnerModelEditor.cs`

2. **XR Settings**: Verify `UnityEngine.XR.XRSettings.enabled` works correctly with Unity 6000's XR plugin system. May need to check XR plugin management API instead.

#### Medium Priority (Editor-Only Tasks)
3. **Shader Compilation**: All shader files need to be validated in Unity Editor
4. **Material Migration**: Materials may need SRP conversion if using URP
5. **Asset Reimport**: All assets must be reimported in Unity Editor after upgrade

### Manual Steps Required (CRITICAL)

After merging this PR, the maintainer MUST perform the following steps in order:

1. **Install Unity 6000.0.58f2**
   - Download from Unity Hub or Unity website
   - Ensure Unity 6000.0.58f2 (exactly) is installed

2. **Open Project in Unity Editor**
   ```
   - Launch Unity Hub
   - Click "Add" and select this project folder
   - Unity will detect the version mismatch and show upgrade dialog
   - Click "Continue" to allow Unity to upgrade the project
   - This may take 10-30 minutes depending on project size
   ```

3. **Let Unity Reimport All Assets**
   ```
   - Unity will automatically reimport assets on first open
   - Monitor Console for errors during reimport
   - Common issues: shader compilation errors, material upgrades
   - DO NOT interrupt this process
   ```

4. **Verify Editor Compilation**
   ```
   - Check Console window for compilation errors
   - All C# scripts should compile successfully
   - Address any API deprecation warnings
   ```

5. **Test Skinner Functionality**
   ```
   - Open test scenes (if any exist)
   - Verify SkinnerSource component works
   - Verify SkinnerDebug visualization
   - Test each Skinner renderer (Particle, Trail, Glitch, Debug)
   ```

6. **Run Play Mode Tests**
   ```
   - Window > General > Test Runner
   - Run all play mode tests
   - Fix any failures related to API changes
   ```

7. **Address BoneWeights API Deprecation**
   ```
   - Search for "#pragma warning disable CS0618" in code
   - Refactor to use new BoneWeights API:
     * Replace source.boneWeights with source.GetAllBoneWeights()
     * Replace mesh.boneWeights = array with mesh.SetBoneWeights()
   - Test thoroughly after refactoring
   ```

8. **Regenerate IDE Project Files**
   ```
   - Assets > Open C# Project (generates/updates .csproj files)
   - Or: Edit > Preferences > External Tools > Regenerate project files
   ```

9. **Test Build Pipeline**
   ```
   - File > Build Settings
   - Select target platform
   - Build and test on target platform
   - Verify no runtime errors
   ```

10. **Update Documentation**
    ```
    - Update README.md with Unity 6000.0.58f2 requirement
    - Update any setup/installation guides
    ```

### Breaking Changes
- **Minimum Unity Version**: Now requires Unity 6000.0.58f2 (was 2017.1.0p3)
- **API Compatibility**: Now uses .NET Standard 2.1 (was .NET 2.0)
- **VR/XR**: VR namespace changed to XR (may affect VR builds)

### Migration Notes
- **From Unity 2017.x**: This is a major upgrade. Extensive testing required.
- **Shader Compatibility**: Built-in render pipeline shaders may need updates for URP if switching
- **Platform Support**: Verify platform-specific APIs still work on target platforms

### Recommended Next Steps
1. Complete manual upgrade steps listed above
2. Address all TODO comments in code
3. Run comprehensive testing on all target platforms
4. Update CI/CD pipeline to use Unity 6000.0.58f2
5. Consider migrating to URP if not already done

### Support
For issues related to this upgrade:
1. Check Unity 6000 migration guide: https://docs.unity3d.com/6000.0/Documentation/Manual/UpgradeGuides.html
2. Review deprecated API documentation
3. Check Unity forum for Unity 6000 specific issues

---

**Note**: This upgrade was performed programmatically. Manual verification and testing in Unity Editor is required before production use.
