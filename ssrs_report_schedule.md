# Get SSRS Report Schedules


SELECT
    c.Name                                  AS ReportName,
   s.Name                                   AS ScheduleName,
    CONVERT(VARCHAR(20), StartDate, 113) AS StartDate,
    CONVERT(VARCHAR(20), EndDate, 113)       AS EndDate,
    CONVERT(VARCHAR(20), NextRunTime, 113)   AS NextRunTime,
    CONVERT(VARCHAR(20), LastRunTime, 113)   AS LastRunTime,
    LastRunStatus                            AS LastRunStatus,
    CASE RecurrenceType
            WHEN  1 THEN 'Once'
            WHEN  2 THEN 'Hourly '
            WHEN  4 THEN 'Daily / Weekly'
            WHEN  6 THEN 'Monthly'
    END                                      AS RecurrenceType,
   EventType                                 AS EventType
FROM dbo.Schedule (NOLOCK) s
INNER JOIN dbo.ReportSchedule rs
   ON s.ScheduleID = rs.ScheduleID
INNER JOIN dbo.Catalog c
   ON rs.ReportID = c.ItemID
ORDER BY c.Name, s.Name
