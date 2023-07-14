## Simple ADS System 
its not perfect but it worked for me so Yeah,



# HOW TO USE
* Attach the script to a game object in your scene that represents the player or the weapon.
* Drag and drop the weapon's transform onto the "Weapon" field in the inspector.
* Drag and drop the transforms for the default position and the running position onto their respective fields in the inspector.
* Drag and drop the transforms for each Aim Down Sights position into the "Aim Down Sights" array field in the inspector.
* Set the "smoothTime" field to adjust the speed of the transition between positions.
* Set the "ADSKey" field to the desired key code for aiming down sights (usually Mouse1).

# Note
I have made some changes to the script, but I have not tested it yet. If you encounter any issues, please let me know.

# Code
```C#
using UnityEngine;
public class ProceduralADS : MonoBehaviour
{
    [Header("Weapon")]
    [SerializeField] private Transform Weapon;
    [SerializeField] private int ActiveADS = 0;
    [Header("Weapon Postions")]
    private Transform _weaponCurrentPosition;
    //Reset Position
    private Transform _weaponDefaultPoition;
    [SerializeField] private Transform _RunningPostion;
    [SerializeField] private Transform[] AimDownSights;
    [Header("Variables")]
    [SerializeField] private float smoothTime = 10;
    [SerializeField] private bool IsRunning = false;
    [SerializeField] private bool IsAiming = false;
    [Header("keys")]
    [SerializeField] private KeyCode ADSKey = KeyCode.Mouse1;
    void OnEnable()
    {
        // Reset Weapon Position
        IsAiming = false;
        _weaponCurrentPosition.localPosition = Vector3.Lerp(_weaponCurrentPosition.localPosition, _weaponDefaultPoition.localPosition, smoothTime * Time.deltaTime);
        _weaponCurrentPosition.localRotation = Quaternion.Lerp(_weaponCurrentPosition.localRotation, _weaponDefaultPoition.localRotation, smoothTime * Time.deltaTime);
    }

    void Update()
    {
        if (IsRunning)
        {
            _weaponCurrentPosition.localPosition = Vector3.Lerp(_weaponCurrentPosition.localPosition, _RunningPostion.localPosition, smoothTime * Time.deltaTime);
            _weaponCurrentPosition.localRotation = Quaternion.Lerp(_weaponCurrentPosition.localRotation, _RunningPostion.localRotation, smoothTime * Time.deltaTime);
        }
        else if (Input.GetKey(ADSKey) && AimDownSights.Length > 1)
        {
            _weaponCurrentPosition.localPosition = Vector3.Lerp(_weaponCurrentPosition.localPosition, _weaponDefaultPoition.localPosition, 0.1f);
            _weaponCurrentPosition.localRotation = Quaternion.Lerp(_weaponCurrentPosition.localRotation, _weaponDefaultPoition.localRotation, 0.1f);
            IsAiming = true;
            _weaponCurrentPosition.localPosition = Vector3.Lerp(_weaponCurrentPosition.localPosition, AimDownSights[ActiveADS].localPosition, smoothTime * Time.deltaTime);
            _weaponCurrentPosition.localRotation = Quaternion.Lerp(_weaponCurrentPosition.localRotation, AimDownSights[ActiveADS].localRotation, smoothTime * Time.deltaTime);

            if (Input.GetAxis("Mouse ScrollWheel") > 0f) // Up | Next ADS Position
            {
                ActiveADS = ActiveADS++ < AimDownSights.Length - 1 ? ActiveADS++ : 0;
            }
            if (Input.GetAxis("Mouse ScrollWheel") < 0f) // Down| Previous ADS Position
            {
                ActiveADS = ActiveADS-- > 0 ? ActiveADS-- : AimDownSights.Length - 1;

            }
            

        }
        else
        {
            _weaponCurrentPosition.localPosition = Vector3.Lerp(_weaponCurrentPosition.localPosition, _weaponDefaultPoition.localPosition, smoothTime * Time.deltaTime);
            _weaponCurrentPosition.localRotation = Quaternion.Lerp(_weaponCurrentPosition.localRotation, _weaponDefaultPoition.localRotation, smoothTime * Time.deltaTime);
        }



        if (Input.GetKeyUp(ADSKey)) IsAiming = false;
    }

    public void StartRunning()
    {
        IsRunning = true;
    }

    public void StopRunning()
    {
        IsRunning = false;
    }
}


```


# Done 



## 📞 Issues
If you have any issues feel free to open an issue or contact me on Discord (sheter).
