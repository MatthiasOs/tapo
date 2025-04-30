# Tapo Status per Cloud-API auslesen
## UUID
beliebige UUID online selber generieren, zb auf https://www.uuidgenerator.net/

## Token von Tapo abfragen
Token hält nur ein paar Minuten!

**Email, Passwort und UUID ersetzen!**
```
curl -X POST \
  "https://eu-wap.tplinkcloud.com" \
  -H "Content-Type: application/json" \
  -d '{
    "method": "login",
    "params": {
      "appType": "Tapo_Android",
      "cloudUserName": "EMAIL",
      "cloudPassword": "PASSWORT",
      "terminalUUID": "UUID"
    }
  }'
```
Antwort enthält den Token:
```
{
	"error_code": 0,
	"result": {
		"accountId": "177321177",
		"regTime": "2024-02-05 14:46:33",
		"countryCode": "DE",
		"riskDetected": 0,
		"nickname": "---redacted---",
		"email": "---redacted---",
		"token": "5a5ef0be---redacted---"
	}
}
```
## Geräteliste abfragen:
**Token ersetzen!**
```
curl -X POST "https://eu-wap.tplinkcloud.com?token=TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"method": "getDeviceList"}'
```
Antwort:
```
{
	"error_code": 0,
	"result": {
		"deviceList": [
			{
				"deviceType": "SMART.TAPOPLUG",
				"role": 0,
				"fwVer": "1.3.1 Build 240621 Rel.162048",
				"appServerUrl": "https://eu-wap.tplinkcloud.com",
				"deviceRegion": "eu-west-1",
				"deviceId": "8022469BE9ABA7E---redacted---",
				"deviceName": "P115",
				"deviceHwVer": "1.0",
				"alias": "U3Rhbm---redacted---",
				"deviceMac": "40---redacted---",
				"oemId": "763B71FDC7202E1AC---redacted---",
				"deviceModel": "P115(EU)",
				"hwId": "6F7160E8939---redacted---",
				"fwId": "00000000000000000000000000000000",
				"isSameRegion": true,
				"appServerUrlV2": "https://eu-wap.tplinkcloud.com",
				"status": 0
			},
			...
	}
}
```
## Geräte Status abfragen
**deviceId aus der Geräteliste oben nehmen**

**Token ersetzen!**
```
  curl -X POST \
  "https://eu-wap.tplinkcloud.com/?token=TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "method": "passthrough",
    "params": {
      "deviceId": "8022469BE9AB---redacted---",
      "requestData": "{\"method\":\"get_device_info\"}"
    }
  }'
```

## Problem: Die Antwort ist immer "offline":
```
{"error_code":-20571,"msg":"Device is offline"}
```
