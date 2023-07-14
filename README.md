## Simple ADS System V2 
its not perfect but it worked for me so Yeah,



# HOW TO USE
* Attach the ProceduralADS script to your Weapon gameobject.
* In the Unity editor, assign the required references in the inspector for the script:
- Assign the WeaponADSLayer transform reference to the appropriate weapon GameObject's transform. This will be the transform that represents the weapon's ADS layer.
- Assign the _camera reference to the Camera component you attached to the GameObject.
* Adjust the values of the variables (smoothTime, offsetX, offsetY, offsetZ, ADSKey) according to your requirements.

# Code
```C#
using UnityEngine;

public class ProceduralADS : MonoBehaviour
{
    [Header("Weapon / Camera")]
    [SerializeField] private Transform WeaponADSLayer;
    [SerializeField] private Camera _camera;

    [Header("Variables")]
    [SerializeField] private float smoothTime = 10f;
    [SerializeField] private float offsetX = 10f;
    [SerializeField] private float offsetY = 10f;
    [SerializeField] private float offsetZ = 10f;
    [SerializeField] private bool IsAiming = false;

    [Header("Keys")]
    [SerializeField] private KeyCode ADSKey = KeyCode.Mouse1;

    private Vector3 originalWeaponPosition; 

    private void Start()
    {
        _camera = GetComponentInParent<Camera>();
        originalWeaponPosition = WeaponADSLayer.localPosition;

        UpdateAiming(false);
    }

    private void Update()
    {
        myInput();
        HandleAiming();
    }

    private void HandleAiming()
    {
        if (IsAiming)
        {
            // Calculate the target screen position at the center of the screen
            Vector3 targetScreenPosition = new Vector3(Screen.width / 2f, Screen.height / 2f, 0f);

            // Convert the screen position to a world position based on the weapon's distance from the camera
            float distanceFromCamera = Vector3.Distance(WeaponADSLayer.position, _camera.transform.position);
            Vector3 targetWorldPosition = _camera.ScreenToWorldPoint(new Vector3(targetScreenPosition.x, targetScreenPosition.y, distanceFromCamera));

            // Convert the world position to a local position relative to the weapon
            Vector3 targetLocalPosition = WeaponADSLayer.parent.InverseTransformPoint(targetWorldPosition);

            // Apply the specified offsets
            targetLocalPosition += new Vector3(offsetX, offsetY, offsetZ);
            targetLocalPosition.z = offsetZ;
            // Lerp the weapon position to match the target position
            WeaponADSLayer.localPosition = Vector3.Lerp(WeaponADSLayer.localPosition, targetLocalPosition, Time.deltaTime * smoothTime);
        }
        else
        {
            WeaponADSLayer.localPosition = Vector3.Lerp(WeaponADSLayer.localPosition, originalWeaponPosition, Time.deltaTime * smoothTime);
        }
    }

    private void myInput()
    {
        if (Input.GetKeyDown(ADSKey))
        {
            UpdateAiming(true);
        }
        if (Input.GetKeyUp(ADSKey))
        {
            UpdateAiming(false);
        }
    }

    private void UpdateAiming(bool Aiming)
    {
        IsAiming = Aiming;
    }
}


```


# Done 



## 📞 Issues
If you have any issues feel free to open an issue or contact me on Discord (sheter).
