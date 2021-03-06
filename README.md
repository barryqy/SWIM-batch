## Edit dnac_config.py first!!!
Edit dnac_config.py with your DNAC IP and credentials first beofre running the following

## How to use upgrade.py
upgrade.py by default will reload the switches one by one
Assume new image is already distributed to the switches through DNA

To command the script to upgrade the switches, provide a list of IPs sepperated by ','.  
make sure to remove spaces, otherwise the shell will get upset. 

Image (--image) is optional, you can leave image blank and the golden image will be used.

- Upgrade by IP address without specifying image
```buildoutcfg
python3 upgrade.py 192.168.0.1,192.168.0.2,192.168.0.3
```
- Upgrade by IP address specifying image (optional)

```buildoutcfg
python3 upgrade.py --image cat9k_iosxe.16.09.05.SPA.bin 192.168.0.1 192.168.0.2 192.168.0.3
```
Or, you can tag the list of switches to be upgraded, 
and use the tag to identify which group of devices to be upgraded

- Upgrade by tag
```buildoutcfg
python3 upgrade.py --tag upgrade9k --image cat9k_iosxe.16.09.05.SPA.bin 
```

## list_images.py
provides a list of all of the images on DNAC

```buildoutcfg
python3 list_images.py 
https://sandboxdnac.cisco.com:8080/api/v1/image/importation
Name                                         Version        FileSize          Family         importSourceType
cat3k_caa-universalk9.16.06.02s.SPA.bin      16.6.2s        409953451 bytes   CAT3K_CAA      FILESYSTEM
cat9k_iosxe.16.06.02s.SPA.bin                16.6.2s        596391064 bytes   CAT9K          FILESYSTEM

```
## Distribute.py
takes a tag and an imagename.  If an image is not supplied, the "golden image" for the device will be used.
Any devices with the tag associated with them will have the image distributed to them.  At present there is no
checking in the script, so you need to be careful (or extend the script) 


```buildoutcfg
python3 /distribute.py --tag upgrade9k --image cat9k_iosxe.16.06.02s.SPA.bin
https://sandboxdnac.cisco.com:8080/api/v1/tag/association?tag=upgrade9k&resourceType=network-device
https://sandboxdnac.cisco.com:8080/api/v1/image/importation?name=cat9k_iosxe.16.06.02s.SPA.bin
https://sandboxdnac.cisco.com:8080/api/v1/image/importation/dbd0f3d3-c24e-4e70-9391-0ab38b3cfc32
CAT9K
https://sandboxdnac.cisco.com:8080/api/v1/network-device/74b69532-5dc3-45a1-a0dd-6d1d10051f27
Cisco Catalyst 9300 Series Switches
Waiting for Task a64bfaea-12f7-442d-9de7-ecede961b226
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
Task=a64bfaea-12f7-442d-9de7-ecede961b226 has not completed yet. Sleeping 60 seconds...
{'version': 1517125988427, 'endTime': 1517125989689, 'startTime': 1517125988389, 'serviceType': 'Swim Service', 'rootId': 'a64bfaea-12f7-442d-9de7-ecede961b226', 'id': 'a64bfaea-12f7-442d-9de7-ecede961b226', 'progress': 'completed successfully.  Success = 1, Failure = 0, Running = 0, Pending = 0, Total = 1', 'additionalStatusURL': '/api/v1/image/task?taskUuid=a64bfaea-12f7-442d-9de7-ecede961b226', 'isError': False, 'lastUpdate': 1517125988427}
https://sandboxdnac.cisco.com:8080/api/v1/image/task?taskUuid=a64bfaea-12f7-442d-9de7-ecede961b226
{
  "version": "1.0",
  "response": [
    {
      "deviceIp": "10.10.22.70",
      "taskType": "distribute",
      "taskStatus": "success",
      "imageName": "cat9k_iosxe.16.06.02s.SPA.bin",
      "operation": "",
      "logDetails": "",
      "taskUuid": "a64bfaea-12f7-442d-9de7-ecede961b226",
      "unitTaskUuid": "1c4f99f8-ef94-4339-87c5-e45a23e2f882",
      "deviceTaskUuid": "20c87cbd-8ea7-46f9-82a8-3f02ace98513",
      "startTime": "1517125988427",
      "deviceId": "74b69532-5dc3-45a1-a0dd-6d1d10051f27",
      "completionTime": "1517125989494"
    }
  ]
}

```
## delete.py
used to delete an image from a device.  This could be used to clean up flash space.  In this case we are using 
it to clean up and allow another image to be pushed.

```buildoutcfg
python3 delete.py --tag upgrade9k --image cat9k_iosxe.16.06.02s.SPA.bin
https://sandboxdnac.cisco.com:8080/api/v1/tag/association?tag=upgrade9k&resourceType=network-device
Waiting for Task 3a455f95-1a87-4ae7-b5a8-c8cd0949fdf8
{'data': 'delete-image-device', 'isError': False, 'startTime': 1517126153695, 'rootId': '3a455f95-1a87-4ae7-b5a8-c8cd0949fdf8', 'id': '3a455f95-1a87-4ae7-b5a8-c8cd0949fdf8', 'progress': 'completed successfully.  Success = 1, Failure = 0, Running = 0, Pending = 0, Total = 1', 'version': 1517126153695, 'endTime': 1517126154725, 'serviceType': 'Swim Service'}

```