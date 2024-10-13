# `PlayerControl` 类
## 定义
```csharp
[Beebyte.Obfuscator.SkipRenameAttribute]
public class PlayerControl : InnerNetObject;
```
-----------------
## 方法
### `AddSystemTask` 方法
#### 介绍
给玩家添加特定任务。
#### 返回结果
所添加任务的`PlayerTask`实例。
#### 定义
```csharp
public PlayerTask AddSystemTask(SystemTypes system);
```
<details>
<summary>点击展示方法体</summary>

```csharp
if (!base.AmOwner)
{
	return null;
}
PlayerTask playerTask = Object.Instantiate<PlayerTask>(ShipStatus.Instance.GetSabotageTask(system), base.transform);
playerTask.Id = 255U;
playerTask.Owner = this;
playerTask.Initialize();
this.myTasks.Add(playerTask);
return playerTask;
```
</details>

### `AdjustLighting` 方法
#### 介绍
设置玩家的灯光。
#### 定义
```csharp
public void AdjustLighting();
```
<details>
<summary>点击展示方法体</summary>

```csharp
if (PlayerControl.LocalPlayer != this)
{
	return;
}
float flashlightSize = 0f;
if (this.IsFlashlightEnabled())
{
	if (this.Data.Role.IsImpostor)
	{
		GameOptionsManager.Instance.CurrentGameOptions.TryGetFloat(FloatOptionNames.ImpostorFlashlightSize, out flashlightSize);
	}
	else
	{
		GameOptionsManager.Instance.CurrentGameOptions.TryGetFloat(FloatOptionNames.CrewmateFlashlightSize, out flashlightSize);
	}
}
this.SetFlashlightInputMethod();
this.lightSource.SetupLightingForGameplay(this.IsFlashlightEnabled(), flashlightSize, this.TargetFlashlight.transform);
```
</details>

### `AllTasksCompleted` 方法
#### 介绍
获得玩家是否完成了所有非破坏任务的一个布尔值。
#### 返回结果
玩家是否完成了所有非破坏任务。
#### 定义
```csharp
public bool AllTasksCompleted();
```
<details>
<summary>点击展示方法体</summary>

```csharp
if (PlayerControl.LocalPlayer != this)
{
	return;
}
float flashlightSize = 0f;
if (this.IsFlashlightEnabled())
{
	if (this.Data.Role.IsImpostor)
	{
		GameOptionsManager.Instance.CurrentGameOptions.TryGetFloat(FloatOptionNames.ImpostorFlashlightSize, out flashlightSize);
	}
	else
	{
		GameOptionsManager.Instance.CurrentGameOptions.TryGetFloat(FloatOptionNames.CrewmateFlashlightSize, out flashlightSize);
	}
}
this.SetFlashlightInputMethod();
this.lightSource.SetupLightingForGameplay(this.IsFlashlightEnabled(), flashlightSize, this.TargetFlashlight.transform);
```
</details>

### `RpcSetScanner` 方法
#### 介绍
显示或隐藏玩家的医疗室扫描动画。

此方法会调用RPC以对其他玩家设置动画。
#### 定义
```csharp
public void RpcSetScanner(bool value);
```
<details>
<summary>点击展示方法体</summary>

```csharp
byte b = this.scannerCount + 1;
this.scannerCount = b;
byte b2 = b;
if (AmongUsClient.Instance.AmClient)
{
	this.SetScanner(value, b2);
}
if (!GameManager.Instance.LogicOptions.GetVisualTasks())
{
	return;
}
MessageWriter messageWriter = AmongUsClient.Instance.StartRpc(this.NetId, 15, 1);
messageWriter.Write(value);
messageWriter.Write(b2);
messageWriter.EndMessage();
```
</details>