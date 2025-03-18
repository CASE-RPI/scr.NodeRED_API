# Node-RED API for Smart Conference Room

Developed by Jihoon Chung
This repository contains Node-RED API codes for accessing sensors and HVAC in the Smart Conference Room at RPI

## Contents

1. [Introduction](#intro)
2. [Getting Started](#manual)
3. [Installation](#setup)
4. [Notes](#note)
5. [Requirements](#requirements)
6. [Contact](#contact)

<a name="intro"></a>
## 1. Introduction

This Node-RED API aims to remotely access real-time data from sensors and HVAC in the Smart Conference Room (SCR). Currently, several problems hinder researchers from collecting the sensor data from the building control system:

1. The sensors and HVAC are only accessible via the local network, 'SCR_AP' WiFi.
2. You cannot collect the data remotely. I found some python scripts in the 'Arunas' server computer for collecting the data by previous researchers; however, it is very inefficient because they needed to move the dataset to their computer manually. Moreover, it could cause an error or technical issue.
3. You are able to access the server via SSH protocol but unable to collect sensor data continuously.

For these reasons, I installed my mini-PC in the SCR and developed a Node-RED API to collect the data remotely. If you want to access the real-time data for your research project, feel free to use my API. This API provides the same functions in the ['scr.scr_control' repository](https://github.com/LESA-RPI/scr.scr_control). Please note that you are unable to control HVAC and lighting via this API to prevent security issues.


<a name="manual"></a>
## 2. Getting Started

#### 2.1. Basic Commands for Running ROS
Before getting started, you need to know the basic command for running ROS. This is the basic rule to run the client, as described in ['scr.scr_control' repository](https://github.com/LESA-RPI/scr.scr_control):

```
  $ rosrun scr_control SCR_[client_name]_client.py [command] [argument]
```

The API users are supposed to provide the information for [client_name], [command], and [argument] via an HTTP POST request using JSON format, as follows:

```json
[POST] http://128.113.122.52:1880/SCR
{
    "client_name": "[client_name]",
    "command": "[command]",
    "arguments": "[argument]"
}
```

Then, the request is converted into a command line by Node-RED, and a sever sends a printout from the corresponding python script. Thus, please make sure that the commands work well on your command prompt.

FYI, RPI VPN is necessary to access this URL because the computer where the Node-RED is installed uses the RPI network.


#### 2.2. Request & Response Sample with Arguments
This is a sample of the request that users are supposed to send to my server computer:
```json
[POST] http://128.113.122.52:1880/SCR
{
    "client_name": "TOF",
    "command": "get_distances",
    "arguments": "1"
}
```
This request lets my server computer run the following command:
```
  $ rosrun scr_control SCR_TOF_client.py get_distances 1
```

If you send the request, you can receive the following response from my server computer.
```json
{
    "result": "[[2712, 3114, 3243, 3225, 3330, 3028, 3243, 3100, 3162, 3028, 3138, 2960, 3187, 3088, 3243, 3114, 3138, 3188, 2589, 2216], [2873, 2966, 3088, 3100, 3162, 3187, 3028, 2972, 3028, 3028, 3083, 3028, 2929, 3083, 3138, 3162, 3187, 3225, 3188, 3629],...",
    "error_code": 0,
    "error_message": ""
}
```

#### 2.3. Request & Response Sample without Arguments
If you provide relevant information based on the guildeline in ['scr.scr_control' repository](https://github.com/LESA-RPI/scr.scr_control), it will works correctly even if you don've provide arguments, as follows:
```json
[POST] http://128.113.122.52:1880/SCR
{
    "client_name": "TOF",
    "command": "get_distances_all",
    "arguments": ""
}
```
This request lets my server computer run the following command:
```
  $ rosrun scr_control SCR_TOF_client.py get_distances_all
```

If you send the request, you can receive the following response from my server computer.
```json
{
    "result": "[[2272, 2246, 2226, 2183, 2164, 2138, 2135, 2137, 2137, 2108, 2088, 2110, 2126, 2132, 2136, 2147, 2202, 2193, 2221, 2264, 2329, 2404, 2313, 2302, 2292, 2321, 2272, 2258, 2301, 2270, 2301, 2282, 2279, 2313, 2307, 2316, 2332, 2377, 2404, 2404, ...",
    "error_code": 0,
    "error_message": ""
}
```


#### 2.4. Example Image
The below figure shows an example of sending the request using Postman. 
<p align="center">
<img src="img/Example%20of%20Request.jpg" alt="Example Image" style="width:70%;"><br><b>Fig 1.</b> Example Image of Request & Response Sample
</p>

Please note that <b> the users are disallowed to control lighting, blinds, and HVAC </b> via this API due to security issues.


<a name="setup"></a>
## 3. Installation

- If you want to edit or customize this API for your project, feel free to download and reuse this Node-RED flow.
- To directly access the sensor and HVAC system, you need a ROS system on your computer. Please read and follow the guidelines in [this repository](https://github.com/LESA-RPI/scr.scr_control).
- After the installation and setting up, install [Node.js](https://nodejs.org/en/download) and [Node-RED](https://nodered.org/docs/getting-started/local) on your computer.
- Clone this repository and import the 'Node-RED API for SCR.json' file to your Node-RED, following [this instruction](https://nodered.org/docs/user-guide/editor/workspace/import-export).
<p align="center">
<img src="img/UI%20of%20scr.Node-RED_API.JPG" alt="Example Image" style="width:70%;"><br><b>Fig 2.</b> UI of Node-RED and imported json file
</p>

<a name="notes"></a>
## 4. Note

- This API was developed only for accessing real-time data from sensors and HVAC in SCR. However, it cannot control HVAC settings, blinds, or lighting for security.
- RPI VPN is required to connect server computers in the SCR from off campus.

<a name="requirements"></a>
## 5. Requirements

- Node.js v18.15.0 (https://nodejs.org/en/download)
- Node-RED v3.0.2 (https://nodered.org/docs/getting-started/local)
- ROS Noetic Ninjemys (http://wiki.ros.org/ROS/Installation) 
- SCR-Control Scripts (https://github.com/LESA-RPI/scr.scr_control)
- RPI VPN (https://itssc.rpi.edu/hc/en-us/articles/360008783172-VPN-Installation-and-Connection)

<a name="contact"></a>
## 5. Contact
If you have any question on this code, feel free to reach out to me ([jihoonchung.research@gmail.com](mailto:jihoonchung.research@gmail.com))
