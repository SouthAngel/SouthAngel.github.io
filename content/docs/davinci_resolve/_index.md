---
---
*吐槽：达芬奇的api真是五脏不全，说是同时支持lua python javascript，只有lua是软件内置的，相当于是另起了一个客户端程序与fuscript通信交互，为此甚至在软件安装目录中带了一整个electorn，插件开发基本上可以说是与达芬奇软件本身没有什么关系了，虽说做到了软件与插件解耦，但会使你的插件不得不使用许多外部依赖，结果就是你想写一个小插件最后也会变的臃肿不堪。这些暂且不论，主要问题是核心api函数许多细节交互都没有，你没办法通过api做一些细致的控制，这个软件对开发者来说体验是很差的。*
## API功能缺失
1. 无法获取用户选择
1. 无法添加右键菜单项
1. 无法获取 In Out 标记
1. 无法通过api控制缩略图刷新
## 代码示例
### 获取时间线
```lua
resolve = Resolve()
projectManager = resolve:GetProjectMannager()
project = projectManager:GetCurrentProject()
timeline = project:GEtCurrentTimeline()
```
## 部分api对象说明
### TimelineItem
```txt
TimelineItem
  GetName()                                       --> string             # Returns the item name.
  GetDuration()                                   --> int                # Returns the item duration.
  GetEnd()                                        --> int                # Returns the end frame position on the timeline.
  GetFusionCompCount()                            --> int                # Returns number of Fusion compositions associated with the timeline item.
  GetFusionCompByIndex(compIndex)                 --> fusionComp         # Returns the Fusion composition object based on given index. 1 <= compIndex <= timelineItem.GetFusionCompCount()
  GetFusionCompNameList()                         --> [names...]         # Returns a list of Fusion composition names associated with the timeline item.
  GetFusionCompByName(compName)                   --> fusionComp         # Returns the Fusion composition object based on given name.
  GetLeftOffset()                                 --> int                # Returns the maximum extension by frame for clip from left side.
  GetRightOffset()                                --> int                # Returns the maximum extension by frame for clip from right side.
  GetStart()                                      --> int                # Returns the start frame position on the timeline.
  SetProperty(propertyKey, propertyValue)         --> Bool               # Sets the value of property "propertyKey" to value "propertyValue"
                                                                         # Refer to "Looking up Timeline item properties" for more information
  GetProperty(propertyKey)                        --> int/[key:value]    # returns the value of the specified key
                                                                         # if no key is specified, the method returns a dictionary(python) or table(lua) for all supported keys
  AddMarker(frameId, color, name, note, duration, --> Bool               # Creates a new marker at given frameId position and with given marker information. 'customData' is optional and helps to attach user specific data to the marker.
            customData)
  GetMarkers()                                    --> {markers...}       # Returns a dict (frameId -> {information}) of all markers and dicts with their information.
                                                                         # Example: a value of {96.0: {'color': 'Green', 'duration': 1.0, 'note': '', 'name': 'Marker 1', 'customData': ''}, ...} indicates a single green marker at clip offset 96
  GetMarkerByCustomData(customData)               --> {markers...}       # Returns marker {information} for the first matching marker with specified customData.
  UpdateMarkerCustomData(frameId, customData)     --> Bool               # Updates customData (string) for the marker at given frameId position. CustomData is not exposed via UI and is useful for scripting developer to attach any user specific data to markers.
  GetMarkerCustomData(frameId)                    --> string             # Returns customData string for the marker at given frameId position.
  DeleteMarkersByColor(color)                     --> Bool               # Delete all markers of the specified color from the timeline item. "All" as argument deletes all color markers.
  DeleteMarkerAtFrame(frameNum)                   --> Bool               # Delete marker at frame number from the timeline item.
  DeleteMarkerByCustomData(customData)            --> Bool               # Delete first matching marker with specified customData.
  AddFlag(color)                                  --> Bool               # Adds a flag with given color (string).
  GetFlagList()                                   --> [colors...]        # Returns a list of flag colors assigned to the item.
  ClearFlags(color)                               --> Bool               # Clear flags of the specified color. An "All" argument is supported to clear all flags.
  GetClipColor()                                  --> string             # Returns the item color as a string.
  SetClipColor(colorName)                         --> Bool               # Sets the item color based on the colorName (string).
  ClearClipColor()                                --> Bool               # Clears the item color.
  AddFusionComp()                                 --> fusionComp         # Adds a new Fusion composition associated with the timeline item.
  ImportFusionComp(path)                          --> fusionComp         # Imports a Fusion composition from given file path by creating and adding a new composition for the item.
  ExportFusionComp(path, compIndex)               --> Bool               # Exports the Fusion composition based on given index to the path provided.
  DeleteFusionCompByName(compName)                --> Bool               # Deletes the named Fusion composition.
  LoadFusionCompByName(compName)                  --> fusionComp         # Loads the named Fusion composition as the active composition.
  RenameFusionCompByName(oldName, newName)        --> Bool               # Renames the Fusion composition identified by oldName.
  AddVersion(versionName, versionType)            --> Bool               # Adds a new color version for a video clip based on versionType (0 - local, 1 - remote).
  GetCurrentVersion()                             --> {versionName...}   # Returns the current version of the video clip. The returned value will have the keys versionName and versionType(0 - local, 1 - remote).
  DeleteVersionByName(versionName, versionType)   --> Bool               # Deletes a color version by name and versionType (0 - local, 1 - remote).
  LoadVersionByName(versionName, versionType)     --> Bool               # Loads a named color version as the active version. versionType: 0 - local, 1 - remote.
  RenameVersionByName(oldName, newName, versionType)--> Bool             # Renames the color version identified by oldName and versionType (0 - local, 1 - remote).
  GetVersionNameList(versionType)                 --> [names...]         # Returns a list of all color versions for the given versionType (0 - local, 1 - remote).
  GetMediaPoolItem()                              --> MediaPoolItem      # Returns the media pool item corresponding to the timeline item if one exists.
  GetStereoConvergenceValues()                    --> {keyframes...}     # Returns a dict (offset -> value) of keyframe offsets and respective convergence values.
  GetStereoLeftFloatingWindowParams()             --> {keyframes...}     # For the LEFT eye -> returns a dict (offset -> dict) of keyframe offsets and respective floating window params. Value at particular offset includes the left, right, top and bottom floating window values.
  GetStereoRightFloatingWindowParams()            --> {keyframes...}     # For the RIGHT eye -> returns a dict (offset -> dict) of keyframe offsets and respective floating window params. Value at particular offset includes the left, right, top and bottom floating window values.
  GetNumNodes()                                   --> int                # Returns the number of nodes in the current graph for the timeline item
  ApplyArriCdlLut()                               --> Bool               # Applies ARRI CDL and LUT. Returns True if successful, False otherwise.
  SetLUT(nodeIndex, lutPath)                      --> Bool               # Sets LUT on the node mapping the node index provided, 1 <= nodeIndex <= total number of nodes.
                                                                         # The lutPath can be an absolute path, or a relative path (based off custom LUT paths or the master LUT path).
                                                                         # The operation is successful for valid lut paths that Resolve has already discovered (see Project.RefreshLUTList).
  GetLUT(nodeIndex)                               --> String             # Gets relative LUT path based on the node index provided, 1 <= nodeIndex <= total number of nodes.
  SetCDL([CDL map])                               --> Bool               # Keys of map are: "NodeIndex", "Slope", "Offset", "Power", "Saturation", where 1 <= NodeIndex <= total number of nodes.
                                                                         # Example python code - SetCDL({"NodeIndex" : "1", "Slope" : "0.5 0.4 0.2", "Offset" : "0.4 0.3 0.2", "Power" : "0.6 0.7 0.8", "Saturation" : "0.65"})
  AddTake(mediaPoolItem, startFrame, endFrame)    --> Bool               # Adds mediaPoolItem as a new take. Initializes a take selector for the timeline item if needed. By default, the full clip extents is added. startFrame (int) and endFrame (int) are optional arguments used to specify the extents.
  GetSelectedTakeIndex()                          --> int                # Returns the index of the currently selected take, or 0 if the clip is not a take selector.
  GetTakesCount()                                 --> int                # Returns the number of takes in take selector, or 0 if the clip is not a take selector.
  GetTakeByIndex(idx)                             --> {takeInfo...}      # Returns a dict (keys "startFrame", "endFrame" and "mediaPoolItem") with take info for specified index.
  DeleteTakeByIndex(idx)                          --> Bool               # Deletes a take by index, 1 <= idx <= number of takes.
  SelectTakeByIndex(idx)                          --> Bool               # Selects a take by index, 1 <= idx <= number of takes.
  FinalizeTake()                                  --> Bool               # Finalizes take selection.
  CopyGrades([tgtTimelineItems])                  --> Bool               # Copies the current grade to all the items in tgtTimelineItems list. Returns True on success and False if any error occurred.
  SetClipEnabled(Bool)                            --> Bool               # Sets clip enabled based on argument.
  GetClipEnabled()                                --> Bool               # Gets clip enabled status.
  UpdateSidecar()                                 --> Bool               # Updates sidecar file for BRAW clips or RMD file for R3D clips.
  GetUniqueId()                                   --> string             # Returns a unique ID for the timeline item
  LoadBurnInPreset(presetName)                    --> Bool               # Loads user defined data burn in preset for clip when supplied presetName (string). Returns true if successful.
  GetNodeLabel(nodeIndex)                         --> string             # Returns the label of the node at nodeIndex.
  CreateMagicMask(mode)                           --> Bool               # Returns True if magic mask was created successfully, False otherwise. mode can "F" (forward), "B" (backward), or "BI" (bidirection)
  RegenerateMagicMask()                           --> Bool               # Returns True if magic mask was regenerated successfully, False otherwise.
  Stabilize()                                     --> Bool               # Returns True if stabilization was successful, False otherwise
  SmartReframe()                                  --> Bool               # Performs Smart Reframe. Returns True if successful, False otherwise.
```
### MediaPoolItem
```txt
MediaPoolItem
  GetName()                                       --> string             # Returns the clip name.
  GetMetadata(metadataType=None)                  --> string|dict        # Returns the metadata value for the key 'metadataType'.
                                                                         # If no argument is specified, a dict of all set metadata properties is returned.
  SetMetadata(metadataType, metadataValue)        --> Bool               # Sets the given metadata to metadataValue (string). Returns True if successful.
  SetMetadata({metadata})                         --> Bool               # Sets the item metadata with specified 'metadata' dict. Returns True if successful.
  GetMediaId()                                    --> string             # Returns the unique ID for the MediaPoolItem.
  AddMarker(frameId, color, name, note, duration, --> Bool               # Creates a new marker at given frameId position and with given marker information. 'customData' is optional and helps to attach user specific data to the marker.
            customData)
  GetMarkers()                                    --> {markers...}       # Returns a dict (frameId -> {information}) of all markers and dicts with their information.
                                                                         # Example of output format: {96.0: {'color': 'Green', 'duration': 1.0, 'note': '', 'name': 'Marker 1', 'customData': ''}, ...}
                                                                         # In the above example - there is one 'Green' marker at offset 96 (position of the marker)
  GetMarkerByCustomData(customData)               --> {markers...}       # Returns marker {information} for the first matching marker with specified customData.
  UpdateMarkerCustomData(frameId, customData)     --> Bool               # Updates customData (string) for the marker at given frameId position. CustomData is not exposed via UI and is useful for scripting developer to attach any user specific data to markers.
  GetMarkerCustomData(frameId)                    --> string             # Returns customData string for the marker at given frameId position.
  DeleteMarkersByColor(color)                     --> Bool               # Delete all markers of the specified color from the media pool item. "All" as argument deletes all color markers.
  DeleteMarkerAtFrame(frameNum)                   --> Bool               # Delete marker at frame number from the media pool item.
  DeleteMarkerByCustomData(customData)            --> Bool               # Delete first matching marker with specified customData.
  AddFlag(color)                                  --> Bool               # Adds a flag with given color (string).
  GetFlagList()                                   --> [colors...]        # Returns a list of flag colors assigned to the item.
  ClearFlags(color)                               --> Bool               # Clears the flag of the given color if one exists. An "All" argument is supported and clears all flags.
  GetClipColor()                                  --> string             # Returns the item color as a string.
  SetClipColor(colorName)                         --> Bool               # Sets the item color based on the colorName (string).
  ClearClipColor()                                --> Bool               # Clears the item color.
  GetClipProperty(propertyName=None)              --> string|dict        # Returns the property value for the key 'propertyName'.
                                                                         # If no argument is specified, a dict of all clip properties is returned. Check the section below for more information.
  SetClipProperty(propertyName, propertyValue)    --> Bool               # Sets the given property to propertyValue (string). Check the section below for more information.
  LinkProxyMedia(proxyMediaFilePath)              --> Bool               # Links proxy media located at path specified by arg 'proxyMediaFilePath' with the current clip. 'proxyMediaFilePath' should be absolute clip path.
  UnlinkProxyMedia()                              --> Bool               # Unlinks any proxy media associated with clip.
  ReplaceClip(filePath)                           --> Bool               # Replaces the underlying asset and metadata of MediaPoolItem with the specified absolute clip path.
  GetUniqueId()                                   --> string             # Returns a unique ID for the media pool item
  TranscribeAudio()                               --> Bool               # Transcribes audio of the MediaPoolItem. Returns True if successful; False otherwise
  ClearTranscription()                            --> Bool               # Clears audio transcription of the MediaPoolItem. Returns True if successful; False otherwise.
```