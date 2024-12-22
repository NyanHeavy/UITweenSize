# UITweenSize
A size control add-on for UITween

🚧 **Requires UITween** 🚧 \
You can get it free [here](https://assetstore.unity.com/packages/tools/animation/ui-tween-38583) in the official unity store

Goal
----
This repository aims to add the possibility to animate the size of the object instead of the scale as default.
![image](https://github.com/user-attachments/assets/a1988634-1d8f-4c91-921e-b552cde68fe3)

Script Modifications
----
For this to be possible, 3 scripts must be modified as shown below.

**EasyTween.cs**
----
> Add line 82
```csharp
public void SetAnimationSize(Vector3 StartSize, Vector3 EndSize, AnimationCurve EntryTween, AnimationCurve ExitTween)
{
    currentAnimationGoing.SetAnimationSize(StartSize, EndSize, EntryTween, ExitTween);
}
```
**EditorUITween.cs**
----
> Add line 22
```csharp
private bool sizeEnabled;
```
> Add line 61
```csharp
EditorSize();
```
> Modify line 91
```csharp
if (positionEnabled || rotationEnabled || scaleEnabled || sizeEnabled || tweenScript.animationParts.FadePropetiesAnim.IsFadeEnabled()) ...
```
> Modify line 103
```csharp
if (positionEnabled || rotationEnabled || scaleEnabled || sizeEnabled) ...
```
> Modify line 122
```csharp
if (positionEnabled || rotationEnabled || scaleEnabled || sizeEnabled || tweenScript.animationParts.FadePropetiesAnim.IsFadeEnabled()) ...
```
> Add line 325
```csharp
void EditorSize()
{
    tweenScript.animationParts.SizePropetiesAnim.SetSizeEnable(EditorGUILayout.BeginToggleGroup("Size Animation",
    tweenScript.animationParts.SizePropetiesAnim.IsSizeEnabled()));
    sizeEnabled = tweenScript.animationParts.SizePropetiesAnim.IsSizeEnabled();

    if (sizeEnabled)
    {
        tweenScript.animationParts.SizePropetiesAnim.StartSize = EditorGUILayout.Vector3Field("Start Size", tweenScript.animationParts.SizePropetiesAnim.StartSize);
        tweenScript.animationParts.SizePropetiesAnim.EndSize = EditorGUILayout.Vector3Field("End Size", tweenScript.animationParts.SizePropetiesAnim.EndSize);
        EditorGUILayout.BeginHorizontal();

        if (tweenScript.animationParts.SizePropetiesAnim.TweenCurveEnterSize == null)
        {
            tweenScript.animationParts.SizePropetiesAnim.TweenCurveEnterSize = new AnimationCurve();
        }
        if (tweenScript.animationParts.SizePropetiesAnim.TweenCurveExitSize == null)
        {
            tweenScript.animationParts.SizePropetiesAnim.TweenCurveExitSize = new AnimationCurve();
        }

        tweenScript.animationParts.SizePropetiesAnim.TweenCurveEnterSize = EditorGUILayout.CurveField("Start Tween Size",
            tweenScript.animationParts.ScalePropetiesAnim.TweenCurveEnterScale);

        EditorGUILayout.Space();

        tweenScript.animationParts.SizePropetiesAnim.TweenCurveExitSize = EditorGUILayout.CurveField("Exit Tween Size",
            tweenScript.animationParts.SizePropetiesAnim.TweenCurveExitSize);

        EditorGUILayout.EndHorizontal();
        EditorGUILayout.Space();
    }
    EditorGUILayout.EndToggleGroup();
}
```
**UITween.cs**
----
> Add line 64
```csharp
if (animationPart.SizePropetiesAnim.IsSizeEnabled())
{
    SizeAnimation(rectTransform, percentage);
}
```
> Add line 232
```csharp
public void SetAnimationSize(Vector2 StartSize, Vector2 EndSize, AnimationCurve EntryTween, AnimationCurve ExitTween)
{
    animationPart.SizePropetiesAnim.StartSize = StartSize;
    animationPart.SizePropetiesAnim.SetSizeEnable(true);
    animationPart.SizePropetiesAnim.EndSize = EndSize;
    animationPart.SizePropetiesAnim.SetAnimationsCurve(EntryTween, ExitTween);
}
```
> Add line 312
```csharp
#region SizeAnim

private AnimationCurve currentAnimationCurveSize;
private Vector3 currentStartSize;
private Vector3 currentEndSize;

public void SetCurrentAnimSize(AnimationCurve currentAnimationCurveSize, Vector3 currentStartSize, Vector3 currentEndSize)
{
    this.currentAnimationCurveSize = currentAnimationCurveSize;
    this.currentStartSize = currentStartSize;
    this.currentEndSize = currentEndSize;
}

public void SizeAnimation(RectTransform _rectTransform, float _counterTween)
{
    float evaluatedValue = currentAnimationCurveSize.Evaluate(_counterTween);
    Vector3 valueAdded = (currentEndSize - currentStartSize) * evaluatedValue;
    _rectTransform.sizeDelta = currentStartSize + valueAdded;
}

#endregion
```
> Add line 508
```csharp
[System.Serializable]
public class SizePropetiesAnim
{
    #region  SizeEditor

    [SerializeField]
    [HideInInspector]
    private bool sizeEnabled;

    public void SetSizeEnable(bool enabled)
    {
        sizeEnabled = enabled;
    }

    public bool IsSizeEnabled()
    {
        return sizeEnabled;
    }

    [HideInInspector]
    public AnimationCurve TweenCurveEnterSize;
    [HideInInspector]
    public AnimationCurve TweenCurveExitSize;
    [HideInInspector]
    public Vector3 StartSize;
    [HideInInspector]
    public Vector3 EndSize;

    public void SetAnimationsCurve(AnimationCurve EntryTween, AnimationCurve ExitTween)
    {
        TweenCurveEnterSize = EntryTween;
        TweenCurveExitSize = ExitTween;
    }

  #endregion
}
```
> Add line 680
```csharp
#region SizeEditor

[HideInInspector]
public SizePropetiesAnim SizePropetiesAnim = new SizePropetiesAnim();

#endregion
```

License
----
**UITweenSize** is available under the **MIT** license. See the LICENSE file for more info.
