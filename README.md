# BGP Route Manipulation Based on BGP state

Slax Script to export certain route groups based on BGP peering status.

The script allows certain defined route groups not be exported based on BGP peering upstream.

Ensure that the export group name is entered into the value below on the script.

var $exportGrpName = "EXPORT_GRP";

Implementation:

The SLAX script can be run in two ways as an:

### 1. Operational Script (OP script)

This is an ondemand way of running the script. To configure an op script ensure the following configuration is added.

    a. Copy the script to the SRX under /var/tmp
    b. Add the following configuration.
       - set system scripts op file /var/tmp/<Script filename>
    c. To run the script, execute the script from operational mode (NOTE: Omit the .slax suffix eg: if the file name test.slax use just test)
       - op <Script Name without .slax>

#### Eg:

set system scripts op file /var/tmp/bgp_export_op.slax

### 2. Event Script

Event scripts run periodically or dynamically based on certain criterias, the configuration below can enable a event script.

#### Step 1: Copy script to SRX and add the script as a event script.
set event-options event-script file <EVENT_SCRIPT>

#### Step 2: Generate a Event on a time interval (Example below, event is generated every 60 secs)
set event-options generate-event <event_name> time-interval 60

#### Step 3: Monitor the generated event in the event policy.
set event-options policy <Event_Policy_Name> events <event_name>
set event-options policy <Event_Policy> then event-script <EVENT_SCRIPT>

#### Eg:
set event-options generate-event new_event time-interval 60

set event-options policy monitor_bgp_peer_and_export events new_event

set event-options policy monitor_bgp_peer_and_export then event-script bgp_export_event.slax

set event-options event-script file bgp_export_event.slax


