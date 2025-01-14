# UITweenSize
A size control add-on for UITween

🚧 **Requires UITween** 🚧 \
You can get it free [here](https://assetstore.unity.com/packages/tools/animation/ui-tween-38583) in the official unity store

Goal
----
This repository aims to add the possibility to animate the size of the object instead of the scale as default.
![image](https://github.com/user-attachments/assets/a1988634-1d8f-4c91-921e-b552cde68fe3)

Disclaimer
----
UITweenSize is not affiliated with, supported or endorsed by the [UITween](https://assetstore.unity.com/packages/tools/animation/ui-tween-38583) asset by [Adi Zhavo](https://assetstore.unity.com/publishers/13979)

Use
----
After importing the asset, modify the following scripts:

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
> Add line 57
```csharp
EditorSize();
```
> Modify line 83
```csharp
if (positionEnabled || rotationEnabled || scaleEnabled || sizeEnabled || tweenScript.animationParts.FadePropetiesAnim.IsFadeEnabled()) ...
```
> Modify line 93
```csharp
if (positionEnabled || rotationEnabled || scaleEnabled || sizeEnabled) ...
```
> Modify line 109
```csharp
if (positionEnabled || rotationEnabled || scaleEnabled || sizeEnabled || tweenScript.animationParts.FadePropetiesAnim.IsFadeEnabled()) ...
```
> Add line 128
```csharp
tweenScript.animationParts.SizePropetiesAnim.StartSize = selectedTransform.sizeDelta;
```
> Add line 138
```csharp
tweenScript.animationParts.SizePropetiesAnim.EndSize = selectedTransform.sizeDelta;
```
> Add line 154
```csharp
if (tweenScript.animationParts.SizePropetiesAnim.IsSizeEnabled())
        selectedTransform.sizeDelta = tweenScript.animationParts.SizePropetiesAnim.StartSize;
```
> Add line 178
```csharp
if (tweenScript.animationParts.SizePropetiesAnim.IsSizeEnabled())
        selectedTransform.sizeDelta = tweenScript.animationParts.SizePropetiesAnim.EndSize;
```
> Add line 304
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
> Add line 147
```csharp
if (animationPart.SizePropetiesAnim.IsSizeEnabled())
{
    SetCurrentAnimSize(animationPart.SizePropetiesAnim.TweenCurveEnterSize,
        animationPart.SizePropetiesAnim.StartSize,
        animationPart.SizePropetiesAnim.EndSize);
}
```
> Add line 200
```csharp
if (animationPart.SizePropetiesAnim.IsSizeEnabled())
{
    SetCurrentAnimSize(animationPart.SizePropetiesAnim.TweenCurveExitSize,
        animationPart.SizePropetiesAnim.EndSize,
        animationPart.SizePropetiesAnim.StartSize);
}
```
> Add line 230
```csharp
public void SetAnimationSize(Vector2 StartSize, Vector2 EndSize, AnimationCurve EntryTween, AnimationCurve ExitTween)
{
    animationPart.SizePropetiesAnim.StartSize = StartSize;
    animationPart.SizePropetiesAnim.SetSizeEnable(true);
    animationPart.SizePropetiesAnim.EndSize = EndSize;
    animationPart.SizePropetiesAnim.SetAnimationsCurve(EntryTween, ExitTween);
}
```
> Add line 303
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
> Add line 521
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
> Add line 693
```csharp
#region SizeEditor

[HideInInspector]
public SizePropetiesAnim SizePropetiesAnim = new SizePropetiesAnim();

#endregion
```

License
----
**UITweenSize** is available under the **MIT** license. See the LICENSE file for more info.
