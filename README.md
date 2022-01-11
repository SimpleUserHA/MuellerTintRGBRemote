# Mueller Tint RGB Remote

This page describes the features and events of the zigbee **Müller Tint RGB Remote** using [ZHA](https://www.home-assistant.io/integrations/zha/) in [Home Assistant](https://www.home-assistant.io/).  
A basic [ZHA quirk](https://github.com/zigpy/zha-device-handlers/tree/dev/zhaquirks/mli) is available at [zigpy](https://github.com/zigpy/zigpy).

Overview
--------

<img src="/assets/Muller_Licht_404011.jpg" alt="Mueller Remote" width="100"/>

On/Off button
--------
Short press on the OnOff button, sends an event after the button was released.  
The **command** is being toggled between the values **on** and **off**.  
There is no long press feature.  

| Button | Description | command |
| :---: | --- | --- |
| ![button_on_off](/assets/button_on_off.png)  | on / off | on <> off |

```json
{
    "event_type": "zha_event",
    "data": {
        "device_ieee": "00:11:22:33:44:55:66:77",
        "unique_id": "00:11:22:33:44:55:66:77:1:0x0005",
        "device_id": "randomdeviceid",
        "endpoint_id": 1,
        "cluster_id": 6,
        "command": command,
        "args": []
    },
    "origin": "LOCAL",
    "time_fired": "yyyy-mm-ddThh:mm:ss.millies+00:00",
    "context": {
        "id": "0b6aa30e10f7b1210b8237a32ed9a0a0",
        "parent_id": null,
        "user_id": null
    }
}
```


Color wheel
--------
**args0** and **args1** are being sent depending on the position of the color wheel.

| Button | Description | args0 | args1 |
| :---: | --- | --- | --- |
| ![button_color_temp_warm](/assets/button_color_temp_warm.png)  | color | 24420 | 38570 |

```json
{
    "event_type": "zha_event",
    "data": {
        "device_ieee": "00:11:22:33:44:55:66:77",
        "unique_id": "00:11:22:33:44:55:66:77:1:0x0005",
        "device_id": "randomdeviceid",
        "endpoint_id": 1,
        "cluster_id": 768,
        "command": "move_to_color",
        "args": [
            args0,
            args1,
            0
        ]
    },
    "origin": "LOCAL",
    "time_fired": "yyyy-mm-ddThh:mm:ss.millies+00:00",
    "context": {
        "id": "f720b665bcb7deb7f587dffbf3012dd2",
        "parent_id": null,
        "user_id": null
    }
}
```


Color temperature
--------
Short press on the color temperature buttons cycles through values defined in table.  
Long press on the color temperature buttons cycles through values defined in table, while keeping it pressed.  
For each value **args0**, a single event is sent.  
*The first value* was set in preparation by the opposite button, but actually no event is sent for the *first value*.  
Once the last value is reached, it is contant and **no** new event is sent.  

| Press type | Button | Description | args0 |
| --- | :---: | --- | --- |
| short press | ![button_color_temp_warm](/assets/button_color_temp_warm.png)  | color temp warm | *153* > 200 > 250 > 370 |
| short press | ![button_color_temp_cold](/assets/button_color_temp_cold.png)  | color temp cold | *370* > 250 > 200 > 153 |
| long press | ![button_color_temp_warm](/assets/button_color_temp_warm.png)  | color temp warm | *153* > 175 > 200 > 222 > 250 > 285 > 333 > 370 > 555 |
| long press | ![button_color_temp_cold](/assets/button_color_temp_cold.png)  | color temp cold | *555* > 370 > 333 > 285 > 250 > 222 > 200 > 175 > 153 |

```json
{
    "event_type": "zha_event",
    "data": {
        "device_ieee": "00:11:22:33:44:55:66:77",
        "unique_id": "00:11:22:33:44:55:66:77:1:0x0005",
        "device_id": "randomdeviceid",
        "endpoint_id": 1,
        "cluster_id": 768,
        "command": "move_to_color_temp",
        "args": [
            args0,
            10
        ]
    },
    "origin": "LOCAL",
    "time_fired": "yyyy-mm-ddThh:mm:ss.millies+00:00",
    "context": {
        "id": "8665e3dff8b1442ca71cdfc98df5aff0",
        "parent_id": null,
        "user_id": null
    }
}
```


Brightness
--------
A short press sends a **step** event with **args** specified in the table.  
A long press sends a **move** event with **args** specified in the table when pressed and a **stop** event when released.

| Press type | Button | Description | command | args |
| --- | :---: | --- | --- | --- |
| short press | ![button_brightness_down](/assets/button_brightness_down.png)  | brightness down | step | 1,43,10 |
| short press | ![button_brightness_up](/assets/button_brightness_up.png)  | brightness up | step | 0,43,10 |
| long press | ![button_brightness_down](/assets/button_brightness_down.png)  | brightness down | move | 1,100 |
| long press | ![button_brightness_up](/assets/button_brightness_up.png)  | brightness up | move | 0,100 |
| release after long press |  |  | stop | *empty* |

```json
{
    "event_type": "zha_event",
    "data": {
        "device_ieee": "00:11:22:33:44:55:66:77",
        "unique_id": "00:11:22:33:44:55:66:77:1:0x0005",
        "device_id": "randomdeviceid",
        "endpoint_id": 1,
        "cluster_id": 8,
        "command": command,
        "args": [args]
    },
    "origin": "LOCAL",
    "time_fired": "yyyy-mm-ddThh:mm:ss.millies+00:00",
    "context": {
        "id": "aa4107fb614b7d9937d9ce590376195c",
        "parent_id": null,
        "user_id": null
    }
}
```


Scenes
--------
Short press on a scene button, sends an event after the button was released.  
There is no long press feature.  
Events for scenes are independently from the current selected group (1-3).  
If **group0** (all three leds) is selected, no event is triggered.

| Button | Description | value |
| :---: | --- | --- |
| ![button_scene_worklight](/assets/button_scene_worklight.png)  | worklight | 3 |
| ![button_scene_sunset](/assets/button_scene_sunset.png)  | sunset | 1 |
| ![button_scene_party](/assets/button_scene_party.png)  | party | 2 |
| ![button_scene_nightlight](/assets/button_scene_nightlight.png)  | nightlight | 6 | 
| ![button_scene_campfire](/assets/button_scene_campfire.png)  | campfire | 4 |
| ![button_scene_romance](/assets/button_scene_romance.png)  | romance | 5 |

```json
{
    "event_type": "zha_event",
    "data": {
        "device_ieee": "00:11:22:33:44:55:66:77",
        "unique_id": "00:11:22:33:44:55:66:77:1:0x0005",
        "device_id": "randomdeviceid",
        "endpoint_id": 1,
        "cluster_id": 5,
        "command": "attribute_updated",
        "args": {
            "attribute_id": 1,
            "attribute_name": "current_scene",
            "value": value
        }
    },
    "origin": "LOCAL",
    "time_fired": "yyyy-mm-ddThh:mm:ss.millies+00:00",
    "context": {
        "id": "0f728c2e8ea142215f83e96cc8b30ece",
        "parent_id": null,
        "user_id": null
    }
}
```

Groups binding
--------------
![button_group_selection](/assets/button_group_selection.png)
-   group0 = All three leds lit (no support for on/off actions and scene activation)
-   group1 = first led lit
-   group2 = second led lit
-   group3 = third let lit


