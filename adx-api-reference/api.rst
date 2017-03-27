**Endpoint**:
https://bpi.automation.api.rackspacecloud.com/2.0/{tenant\_id}/loadbalancers

{device\_id}
============

.. contents::
	:depth: 1
	:local: 

Retrieve device information
---------------------------

Use the device ID operation to get complete information about the device
with the specified ID including associated customer, usage statistics,
and configuration details for nodes, virtual IPs, and high availability.

.. code:: javascript

    GET /{device_id}

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "load_balancer_data": {
        "customer": "1234567",
        "uptime": "unimplemented",
        "hostname": "adx1000-examplehost.iad3.netdev.net",
        "ha_role": "high",
        "ram_mem": [
          {
            "total_kbytes": "2097152",
            "free_kbytes": "1339548",
            "name": "MP",
            "used_kbytes": "757604"
          },
          {
            "total_kbytes": "458752",
            "free_kbytes": "348964",
            "name": "BP1",
            "used_kbytes": "109788"
          }
        ],
        "id": "1234567",
        "os_version": "127.0.00sT403",
        "management_ip": "127..0.0.1",
        "role": "unimplemented",
        "cpu_load": [
          {
            "mod_name": "MASTER_CPU",
            "load_5_sec_avg": {
              "percent_load": 1,
              "seconds_since": 5
            }
          },
          {
            "mod_name": "ASB1/1",
            "load_5_sec_avg": {
              "percent_load": 1,
              "seconds_since": 5
            }
          },
          {
            "mod_name": "ASB1/2",
            "load_5_sec_avg": {
              "percent_load": 1,
              "seconds_since": 5
            }
          },
          {
            "mod_name": "ASB1/3",
            "load_5_sec_avg": {
              "percent_load": 1,
              "seconds_since": 5
            }
          },
          {
            "mod_name": "ASB1/4",
            "load_5_sec_avg": {
              "percent_load": 1,
              "seconds_since": 5
            }
          }
        ],
        "ha_status": "none",
        "model_name": "SI-1216-4-EXAMPLE"
      }
    }

Show high availability template
-------------------------------

Retrieves the high availability configuration template for a device with
the specified ID.

.. code:: javascript

    GET /{device_id}/ha

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "message": "This is a test template for High Availability"
    }

Retrieve virtual IPs configuration
----------------------------------

Load balancers must have at least one virtual IP address that clients
can use to balance traffic across nodes. You can use the manage virtual
IPs operations to configure and manage the virtual IP addresses for the
load balancer with the specified device ID.

An IP can be passed into the add Virtual IP call as part of the request
body, only if the IP exists within an existing Virtual.

*When adding a Virtual IP, these fields are required: account\_number,
label, protocol, port, algorithm, persistence, admin\_state, comment*

.. code:: javascript

    GET /{device_id}/vips

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "vips": [
        {
          "protocol": "TCP",
          "description": "",
          "algorithm": {
            "name": "LEAST_CONNECTION",
            "persistence": null
          },
          "ip": "127.0.0.1",
          "runtime_state": "UNHEALTHY",
          "label": "Vip-Test-32fce25d",
          "port_number": 80,
          "port_name": "HTTP",
          "admin_state": "ENABLED",
          "stats": {
            "conn_max": -1,
            "pkts_out": -1,
            "bytes_in": -1,
            "pkts_in": 0,
            "conn_tot": 0,
            "conn_cur": 0,
            "bytes_out": -1
          },
          "nodes": [
            {
              "label": "Node-Test-32fce25d",
              "port_name": "HTTP",
              "address": "127.0.0.1",
              "port_number": 80,
              "id": "Node-Test-32fce25d:127.0.0.1:80"
            },
            {
              "label": "Node-Test-8df4d3b7",
              "port_name": "HTTP",
              "address": "127.0.0.1",
              "port_number": 80,
              "id": "Node-Test-8df4d3b7:127.0.0.1:80"
            }
          ],
          "id": "Vip-Test-32fce25d:127.0.0.1:80",
          "vendor_extensions": {
            "none": "none"
          }
        }
      ]
    }

Add a Virtual IP

.. code:: javascript

    POST /{device_id}/vips

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)",
      "label": "<Label> (required)",
      "description": "<description>",
      "ip": "<ip>",
      "protocol": "<protocol> (required)",
      "port": "<port> (required)",
      "algorithm": {} (required),
      "persistence": {} (required),
      "nodes": {},
      "admin_state": "<enabled|disabled> (required)",
      "comment": "comment (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Retrieve virtual IP information
-------------------------------

Use the virtual IPs information operations to retrieve and update
information for a virtual IP configured for the specified device ID.

Use the delete operation to remove a virtual IP from the device
configuration.

If you don't know the ID for a specified virtual IP, use the retrieve
virtual IPs operation to find it.

*When deleting, these fields are required: account\_number, comment*

.. code:: javascript

    GET /{device_id}/vips/{vip_id}

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "load_balancer_data": {
        "protocol": "TCP",
        "description": "Some description",
        "algorithm": {
          "persistence_method": "client_ip",
          "name": "LEAST_CONNECTION",
          "persistence": "ENABLED",
          "subnet_prefix_length": 0
        },
        "ip": "127.0.0.1",
        "runtime_state": "UNHEALTHY",
        "label": "Vip-Test-32fce25d",
        "port_number": 80,
        "port_name": "HTTP",
        "admin_state": "ENABLED",
        "stats": {
          "conn_max": -1,
          "pkts_out": -1,
          "bytes_in": -1,
          "pkts_in": 0,
          "conn_tot": 0,
          "conn_cur": 0,
          "bytes_out": -1
        },
        "nodes": [
          {
            "label": "Node-Test-32fce25d",
            "port_name": "HTTP",
            "address": "127.0.0.1",
            "port_number": 80,
            "id": "Node-Test-32fce25d:127.0.0.1:80"
          }
        ],
        "id": "Vip-Test-32fce25d:127.0.0.1:80",
        "vendor_extensions": {
          "none": "none"
        }
      }
    }

Update virtual IP information
-----------------------------

Use the virtual IPs information operations to retrieve and update
information for a virtual IP configured for the specified device ID.

Use the delete operation to remove a virtual IP from the device
configuration.

If you don't know the ID for a specified virtual IP, use the retrieve
virtual IPs operation to find it.

*The following fields are required when you delete a virtual IP,
account\_number, comment*

.. code:: javascript

    PUT /{device_id}/vips/{vip_id}

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)",
      "label": "<Label> (required)",
      "description": "<description>",
      "ip": "<ip>",
      "protocol": "<protocol> (required)",
      "port": "<port> (required)",
      "algorithm": {} (required),
      "persistence": {} (required),
      "nodes": {},
      "admin_state": "<enabled|disabled> (required)",
      "comment": "comment (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Delete a virtual IP
-------------------

Use the virtual IPs information operations to retrieve and update
information for a virtual IP configured for the specified device ID.

Use the delete operation to remove a virtual IP from the device
configuration.

If you don't know the ID for a specified virtual IP, use the retrieve
virtual IPs operation to find it.

*The following fields are required for the delete operation:
account\_number, comment*

.. code:: javascript

    DELETE /{device_id}/vips/{vip_id}

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)",
      "comment": "<comment> (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

List nodes for the specified virtual IP
---------------------------------------

Retrieve information about the nodes associated with a specified virtual
IP.

.. code:: javascript

    GET /{device_id}/vips/{vip_id}/nodes

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "load_balancer_data": [
        {
          "label": "Node-Test-32fce25d",
          "port_name": "HTTP",
          "address": "127.0.0.1",
          "port_number": 80,
          "id": "Node-Test-32fce25d:29.181.84.2:80"
        }
      ]
    }

Assign node to Virtual IP
-------------------------

Use the virtual IP node configuration operations to add or remove a
specified node from the virtual IP configuration.

*When you assign a node to a virtual IP, the following field is
required: account\_number.*

.. code:: javascript

    POST /{device_id}/vips/{vip_id}/nodes/{node_id}

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number>"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Remove node from Virtual IP configuration
-----------------------------------------

Use the virtual IP node configuration operations to add or remove a
specified node from the virtual IP configuration.

.. code:: javascript

    DELETE /{device_id}/vips/{vip_id}/nodes/{node_id}

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Enable the Virtual IP
---------------------

Use the virtual IP configuration operations to enable or disable a
virtual IP configured for a specified device.

.. code:: javascript

    POST /{device_id}/vips/{vip_id}/configuration

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Deactivate the Virtual IP
-------------------------

Use the virtual IP configuration operations to enable or disable a
virtual IP configured for a specified device.

.. code:: javascript

    DELETE /{device_id}/vips/{vip_id}/configuration

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Show Virtual IP statistics

.. code:: javascript

    GET /{device_id}/vips/{vip_id}/stats

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
        "load_balancer_data": {
            "conn_max": -1,
            "pkts_out": -1,
            "bytes_in": -1,
            "pkts_in": 0,
            "conn_tot": 0,
            "conn_cur": 0,
            "bytes_out": -1
        }
    }

Nodes in a device for the given device id
-----------------------------------------

A node is a back-end device providing a service on a specified IP and
port.

Use the nodes operations to get information about the nodes configured
for a specified device and to add a node.

After a node has been defined, use the virtual IP nodes configuration
operations to assign the node to one or more virtual IPs.

*When adding a node to a device, these fields are rquired:
account\_number, label, ip, port, admin\_state, health\_strategy,
vendor\_extensions, comment*

.. code:: javascript

    GET /{device_id}/nodes

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "load_balancer_data": [
        {
          "stats": {
            "conn_max": 0,
            "pkts_out": 0,
            "bytes_in": 0,
            "pkts_in": 0,
            "conn_tot": 0,
            "conn_cur": 0,
            "bytes_out": 0
          },
          "runtime_state": "UNHEALTHY",
          "label": "Node-Test-c4b3b8a5",
          "port_name": "12345",
          "admin_state": "ENABLED",
          "address": "127.0.0.1",
          "port_number": 12345,
          "id": "Node-Test-c4b3b8a5:29.235.243.3:12345"
        }
      ]
    }

Add a node to a device
----------------------

A node is a back-end device providing a service on a specified IP and
port.

Use the nodes operations to get information about the nodes configured
for a specified device and to add a node.

After a node has been defined, use the virtual IP nodes configuration
operations to assign the node to one or more virtual IPs.

*When adding a node to a device, the following fields are required:
account\_number, label, ip, port, admin\_state, health\_strategy,
vendor\_extensions, comment*

.. code:: javascript

    POST /{device_id}/nodes

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)",
      "label": "<Node Label> (required)",
      "description": "<description>",
      "ip": "<ip> (required)",
      "port": "<port> (required)",
      "admin_state": "<enabled|disabled> (required)",
      "health_strategy": "<health_strategy JSON Object> (required)",
      "vendor_extensions": "<vendor_extension JSON object> (required)",
      "comment": "comment (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Retrieve Node information
-------------------------

Use the node operations to view, update, or remove a specified node.

.. code:: javascript

    GET /{device_id}/nodes/{node_id}

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "load_balancer_data": {
        "protocol": "TCP",
        "description": null,
        "runtime_state": "UNHEALTHY",
        "label": "Node-Test-c4b3b8a5",
        "port_name": "12345",
        "port_number": 12345,
        "limit": 1000,
        "admin_state": "ENABLED",
        "address": "127.0.0.1",
        "stats": {
          "conn_max": 0,
          "pkts_out": 0,
          "bytes_in": 0,
          "pkts_in": 0,
          "conn_tot": 0,
          "conn_cur": 0,
          "bytes_out": 0
        },
        "id": "Node-Test-c4b3b8a5:127.0.0.1.3:12345",
        "vendor_extensions": {
          "reassign_count": 0
        },
        "health_strategy": {
          "http_body_pattern": null,
          "http_codes_ok": [
            200,
            203
          ],
          "ssl": false,
          "port_number": 12345,
          "path": "/",
          "strategy": "HTTP_RES_CODE",
          "method": "GET"
        }
      }
    }

Update node information
-----------------------

Use the node operations to view, update, or remove a specified node.

.. code:: javascript

    PUT /{device_id}/nodes/{node_id}

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)",
      "ip": "<ip>",
      "port": "<port>",
      "label": "<Node Label>",
      "health_strategy": {},
      "admin_state": "<enabled|disabled>"
      "vendor_extensions": {},
      "comment": "<comment> (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Delete the Node
---------------

Delete node from the device configuration

.. code:: javascript

    DELETE /{device_id}/nodes/{node_id}

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Enable a node
-------------

Use the node status operations to enable or disable a specified node
included in the device configuration.

If you want to delete the node from the configuration file, use the
delete node operation.

.. code:: javascript

    POST /{device_id}/nodes/{node_id}/configuration

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Disable a node
--------------

Use the node status operations to enable or disable a specified node
included in the device configuration.

If you want to delete the node from the configuration file, use the
delete node operation.

.. code:: javascript

    DELETE /{device_id}/nodes/{node_id}/configuration

Request body:
^^^^^^^^^^^^^

.. code:: javascript

    {
      "account_number": "<Account Number> (required)"
    }

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

Show node statistics
--------------------

Retrieves usage data for a specified node ID.

.. code:: javascript

    GET /{device_id}/nodes/{node_id}/stats

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "load_balancer_data": {
        "conn_max": 0,
        "pkts_out": 0,
        "bytes_in": 0,
        "pkts_in": 0,
        "conn_tot": 0,
        "conn_cur": 0,
        "bytes_out": 0
      }
    }

List Events
-----------

Use the events operations to get information about requests to create or
modify load balancer resources.

.. code:: javascript

    GET /{device_id}/events

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "data": [
        {
          "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
          "@type": "Event",
          "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
          "status": "200",
          "message": "Processing",
          "timestamp": "2015-04-01T10:05:01.55Z",
        },
        {
          "@id": "/loadbalancers/0a68f7c8-e2f9-11e4-8a00-1681e6b88ec1",
          "@type": "Event",
          "event_id": "0a68f7c8-e2f9-11e4-8a00-1681e6b88ec1",
          "status": "202",
          "message": "Accepted",
          "timestamp": "2015-04-01T11:17:05.45Z",
        },
        {
          "@id": "/loadbalancers/104e8b58-e2f9-11e4-8a00-1681e6b88ec1",
          "@type": "Event",
          "event_id": "104e8b58-e2f9-11e4-8a00-1681e6b88ec1",
          "status": "201",
          "message": "Created",
          "timestamp": "2015-04-01T19:15:01.3Z",
        }
      ]
    }

Retrieves event information by event ID
---------------------------------------

Use the event ID details operation to get information about about a
specific event including event type, status, message, and timestamp.

.. code:: javascript

    GET /{device_id}/events/{event_id}

*Resource does not require a body*

Response:
^^^^^^^^^

.. code:: javascript


    {
      "@id": "/loadbalancers/0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "@type": "Event",
      "event_id": "0a68f566-e2f9-11e4-8a00-1681e6b88ec1",
      "status": "200",
      "message": "Processing",
      "timestamp": "2015-04-01T10:05:01.55Z",
    }

