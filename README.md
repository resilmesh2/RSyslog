# Wazuh - RSyslog

For the sake of simplicity, we will leverage the RSyslog server within Wazuh to send the events from the Resilmesh framework, since the RSyslog is automatically collected
by Wazuh, so no wazuh agent will be needed.

Add [resilmesh.conf](./resilmesh.conf) to /etc/rsyslog.d/ directory in your Wazuh server, this will open a TCP port 10514 which will be used by Vector
to send the events.

Restart RSyslog with `systemctl restart rsyslog`

Test if RSyslog port is opened with `telnet localhost 10514`, type anything and check if outputs to /var/log/resilmesh.log

For Wazuh be able to understand the new logs generated, we need to add a localfile directive into `/var/ossec/etc/ossec.conf`,
go to https://localhost:4343/app/settings#/manager/?tab=configuration and add the following between <ossec_config> directive,
should be at the end of the file:
```xml
<ossec_config>
<!-- there might be other configurations around here, don't change them -->
    <localfile>
        <log_format>syslog</log_format>
        <location>/var/log/resilmesh.log</location>
    </localfile>
<!-- there might be other configurations around here, don't change them -->
</ossec_config>
```