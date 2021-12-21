    // List computers that have no heartbeat in last 30 minutes
    Heartbeat 
    | summarize LastHeartbeat=max(TimeGenerated) by Computer 
    | where LastHeartbeat < ago(30m)
