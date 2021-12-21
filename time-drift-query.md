    Event
    | where EventLog == "System" and Source == "Microsoft-Windows-Time-Service" and EventLevelName == "Error"
    | project Computer, RenderedDescription, TimeGenerated
