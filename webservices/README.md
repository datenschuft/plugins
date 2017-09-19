# WebServices plugin

## Description

This plugin makes use of the new SmartHomeNG module system. It provides a Webservice API based on REST and is 
built upon Cherrpy.
Basic REST command such as PUT and GET are supported.

## Requirements

This plugin requires CherryPy to be installed via pip.

## Configuration

### plugin.conf (deprecated) / plugin.yaml

```
[WebServices]
   class_name = WebServices
   class_path = plugins.webservices
```

```yaml
WebServices:
    class_name: WebServices
    class_path: plugins.webservices
```

### items.conf (deprecated) / items.yaml

Currently access to all items is provided via the REST api, no setting has to be made in items.conf (deprecated) / items.yaml

## Usage

### General: Error and Success Messages

In case of an error (e.g. item is not found), the plugin returns an error formatted as JSON:

{"Error": "No item with item path offifce.light found."}

In case a request is successful, it returns a SUCCESS message as JSON.

### Simple Interface

#### Get Value

Gets the data of an item, enriched by meta data, as json object.

http://<your_server_ip>:<your_backend_port>/ws/items/<item_path>

E.g. http://192.168.178.100:1234/ws/items/office.light retuns:

{"name": "office.light", "last_update": "2017-08-20 16:53:01.914540+02:00", "path": "office.light", "eval": "None", "previous_age": "", "enforce_updates": "False", "threshold": "False", "previous_change": "2017-08-20 17:03:18.041298+02:00", "changed_by": "Cache", "logics": ["LightCheckLogic"], "last_change": "2017-08-20 16:53:01.914540+02:00", "triggers": ["bound method KNX.update_item of plugins.knx.KNX", "bound method WebSocket.update_item of plugins.visu_websocket.WebSocket", "bound method Simulation.update_item of plugins.simulation.Simulation"], "previous_value": true, "type": "bool", "cycle": "", "autotimer": "False", "cache": "/python/smarthome_dev/var/cache/office.light", "config": {"alexa_actions": "turnOn turnOff", "alexa_name": "Lampe B\u00fcro", "knx_dpt": "1", "knx_init": "2/3/50", "knx_listen": "2/3/50", "knx_send": ["2/3/10"], "nw": "yes", "sim": "track", "visu_acl": "rw"}, "age": 10441.073385, "value": true, "eval_trigger": "False", "crontab": ""}

#### Set Value

Sets a value of an item.

http://<your_server_ip>:<your_backend_port>/items/<item_path>/<value>

E.g. http://192.168.178.100:1234/ws/items/office.light/0 or http://192.168.178.100:1234/ws/items/office.light/False turns off the light.

### REST Compliant Interface

#### HTTP GET (e.g. normal access to the URL)

##### Reading an Item's Values

Gets the value of an item, enriched by meta data, as json object. Here, also the REST Url is provided as URL field.

http://<your_server_ip>:<your_backend_port>/rest/items/<item_path>

E.g. http://192.168.178.100:1234/rest/items/office.light 

returns

{"value": true, "config": {"alexa_actions": "turnOn turnOff", "alexa_name": "Lampe B\u00fcro", "knx_dpt": "1", "knx_init": "2/3/50", "knx_listen": "2/3/50", "knx_send": ["2/3/10"], "nw": "yes", "sim": "track", "visu_acl": "rw"}, "previous_age": "", "crontab": "", "last_change": "2017-08-20 16:53:01.914540+02:00", "previous_change": "2017-08-20 19:49:54.424230+02:00", "cycle": "", "enforce_updates": "False", "name": "office.light", "threshold": "False", "age": 10631.105385, "triggers": ["bound method KNX.update_item of plugins.knx.KNX", "bound method WebSocket.update_item of plugins.visu_websocket.WebSocket", "bound method Simulation.update_item of plugins.simulation.Simulation"], "type": "bool", "eval_trigger": "False", "autotimer": "False", "logics": ["LightCheckLogic"], "changed_by": "Cache", "cache": "/python/smarthome_dev/var/cache/office.light", "path": "office.light", "last_update": "2017-08-20 16:53:01.914540+02:00", "url": "http://192.168.178.100:1234/rest/items/office.light", "previous_value": true, "eval": "None"}

##### Item List

The following URL prints out a list of all items, that can be requested or modified by the plugin (all str, num and bool items).
For each item, the detail information is also delivered.

http://<your_server_ip>:<your_backend_port>/rest/items/

E.g. http://192.168.178.100:1234/rest/items/ returns the list of all available (str, num, bool) items.

#### HTTP PUT

A HTTP PUT request to the URL sets a value of an item. Only num, bool and str item types are supported.
For bool items you can use int values 0 and 1, but also "yes", "no", "y", "n", "true", "false", "t", "f", "on", "off".
In case you send a string (or a string bool representation), take care it is provided in "...".

http://<your_server_ip>:<your_backend_port>/rest/items/<item_path>

E.g. a PUT request with 0 as payload to http://192.168.178.100:1234/rest/items/office.light turns off the light.