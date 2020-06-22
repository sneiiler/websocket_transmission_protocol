# websocket_transmission_protocol
雷达、电机的通信协议

---
### 约定

通信协议包含两字段，分别为：

`data_type` 数据指令类型，一般由上位机发送，下位机接收。

`data` 数据体，这里是具体要发送的指令内容或者数据内容。

---

### 雷达通信协议

#### 1. 下位机接收来自上位机的 ROI 范围指定指令

**指令说明：** 上位机发送指令，指明雷达输出数据的ROI区域范围。配合雷达的ROI识别程序使用
```
{
    "data_type":"LIDAR-COMMAND-SINGLE-ROI-AREA",
    "data":{
        "ROI_angle_min":20,
        "ROI_angle_max":25,
        "ROI_distance_min":50,
        "ROI_distance_max":60
    }
}
```
字段说明：

`data_type` : 数据指令类型
> " LIDAR-COMMAND-SINGLE-ROI-AREA " : 该指令为设定雷达输出数据的ROI区域范围。


`data` : 数据体

> `ROI_angle_min` ROI区域的最小角度，`type : float/int`
>
> `ROI_angle_max` ROI区域的最大角度，`type : float/int`
>
> `ROI_distance_min` ROI区域的最小距离，`type : float/int`
>
> `ROI_distance_max` ROI区域的最大距离，`type : float/int`



#### 2. 下位机（雷达）发送原始数据到上位机

**指令说明：** 下位机（雷达）发送数据到上位机，输出原始扫描数据。配合雷达的串口透传程序 ` client_upload_origin_data.py ` 使用。

```
{
    "data_type": "LIDAR-DATA-ORIGIN-DATA",
    "data": {
        "serialnumber-xxx":[ [angle, distance], [angle, distance], ..., [angle, distance] ]
    }
}
```

字段说明：

`data_type` : 数据指令类型
> " LIDAR-DATA-ORIGIN-DATA " : 该指令为下位机（雷达）发送数据到上位机，输出原始扫描数据。

`data` : 数据体

> `serialnumber-xxx` 雷达序列号， ***注意：*** 这不是一个固定名称的字段，在实际数据传输过程中会替换为实际的雷达序列号，例如： `DEDE9AF2C1EA9FC2A2EB92F14D0D3C00`
>> 数据内容为`[角度, 距离]` 列表。

实际数据帧示例如下：

```
{
    "data_type": "LIDAR-DATA-ORIGIN-DATA",
    "data": {
        "DEDE9AF2C1EA9FC2A2EB92F14D0D3C00":[ [1.1, 200], [2.3, 201.5], ..., [359.2, 100.9] ]
    }
}
```

#### 3. 下位机发送ROI数据到上位机

**指令说明：** 下位机发送数据到上位机，雷达输出ROI数据。配合雷达的ROI识别 ` client_detect_single_ROI.py ` 使用。
```
{
    "data_type": "LIDAR-DATA-SINGLE-ROI-AREA",
    "data": {
        "serialnumber-xxx":[ [ROI_angle, ROI_distance], [ROI_angle, ROI_distance], ..., [ROI_angle, ROI_distance] ]
    }
}
```

字段说明：

`data_type` : 数据指令类型
> " LIDAR-DATA-SINGLE-ROI-AREA " : 该指令为下位机（雷达）发送数据到上位机，输出ROI数据。

`data` : 数据体

> `serialnumber-xxx` 雷达序列号， ***注意：*** 这不是一个固定名称的字段，在实际数据传输过程中会替换为实际的雷达序列号，例如： `DEDE9AF2C1EA9FC2A2EB92F14D0D3C00`
>> 数据内容为`[角度, 距离]` 列表。

实际数据帧示例如下（例如ROI参数： ROI_angle : [1, 6]，ROI_distance : [100, 220]）

```
{
    "data_type": "LIDAR-DATA-SINGLE-ROI-AREA",
    "data": {
        "DEDE9AF2C1EA9FC2A2EB92F14D0D3C00":[ [1.1, 120], [2.3, 201.5], [5.2, 130.9] ]
    }
}
```