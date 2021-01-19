# SQL_codes
Analyzing Busines Data

Recently, the AEMR management team has been increasingly aware of a large number
of energy providers that submitted outages over the 2016 and 2017 calendar years. The
management team has expressed a desire to have the following two areas of concern
addressed:

A) Energy Stability and Market Outages

B) Energy Losses and Market Reliability 


EXTRACTION CODES FOR ANALYSIS

SELECT COUNT(*) AS Total_Number_Outage_Events,Status, Reason
FROM AEMR 
WHERE YEAR(Start_Time) = 2016 AND Status = 'Approved'
GROUP BY Status, Reason
ORDER BY Reason;

SELECT COUNT(*) AS Total_Number_Outage_Events, Status,Reason
FROM AEMR 
WHERE YEAR(Start_Time) = 2017 AND Status = 'Approved'
GROUP BY Reason
ORDER BY Reason;

SELECT Status,Reason,COUNT(*) AS Total_Number_Outage_Events,
ROUND(AVG(ROUND((TIMESTAMPDIFF(MINUTE, Start_Time, End_Time)/60)/24,2)),2) AS Average_Outage_Duration_Time_Days,
YEAR(Start_Time) AS Year
FROM AEMR
WHERE Status = 'Approved'
GROUP BY Status,Reason,Year
ORDER BY Reason,Year;


SELECT Status,Reason,COUNT(*) AS Total_Number_Outage_Events,MONTH(Start_Time) AS Month
FROM AEMR 
WHERE YEAR(Start_Time) = 2016
AND Status = 'Approved'
GROUP BY Reason,Status,Month
ORDER BY Reason,Month;

SELECT Status,COUNT(*) AS Total_Number_Outage_Events,MONTH(Start_Time) AS Month, YEAR(Start_Time) AS Year
FROM AEMR 
WHERE Status = 'Approved'
GROUP BY Status,Month,Year
ORDER BY Month,Year;

SELECT	Status,Count(*) as Total_Number_Outage_Events,Month(Start_Time) as Month,Year(Start_Time) as Year
FROM AEMR
WHERE Status='Approved'
GROUP BY Status, Month(Start_Time),Year(Start_Time)
ORDER BY Year(Start_Time), Month(Start_Time)


SELECT COUNT(*) AS Total_Number_Outage_Events,Participant_Code,Status,YEAR(Start_Time) AS Year
FROM AEMR
WHERE Status = 'Approved'
GROUP BY Participant_Code,Status,Year
ORDER BY Year,Participant_Code;


SELECT Participant_Code, Status,YEAR(Start_Time) AS Year, ROUND(AVG(ROUND((TIMESTAMPDIFF(MINUTE, Start_Time, End_Time)/60)/24,2)),2) AS Average_Outage_Duration_Time_Days
FROM AEMR
WHERE Status = 'Approved'
GROUP BY Participant_Code, Status, Year
ORDER BY Year, Average_Outage_Duration_Time_Days DESC;

SELECT COUNT(*) AS Total_Number_Outage_Events,Reason,YEAR(Start_Time) AS Year
FROM AEMR
WHERE Status= 'Approved' AND Reason = 'Forced'
GROUP BY Reason, Year
ORDER BY Reason, Year;


SELECT
	SUM(CASE WHEN Reason = 'Forced' THEN 1 ELSE 0 END) as Total_Number_Forced_Outage_Events
	,Count(*) as Total_Number_Outage_Events
	,CAST((CAST(SUM(CASE WHEN Reason = 'Forced' THEN 1 ELSE 0 END)AS DECIMAL(18,2))/CAST(Count(*) AS DECIMAL(18,2)))*100 AS DECIMAL(18,2)) as Forced_Outage_Percentage
	,Year(Start_Time) as Year
FROM
	AEMR
WHERE
	Status = 'Approved'
GROUP BY
	Year(Start_Time)



SELECT Participant_Code,Facility_Code,Status, YEAR(Start_Time) AS Year,ROUND(AVG(Outage_MW),2) AS Avg_Outage_MW_Loss, ROUND(SUM(Outage_MW),2) AS Summed_Energy_Lost
FROM AEMR
WHERE Status = 'Approved' AND Reason = 'Forced'
GROUP BY Participant_Code,Facility_Code,Status,Year
ORDER BY Year,Summed_Energy_Lost DESC;




SELECT Participant_Code, Status,YEAR(Start_Time) AS Year, ROUND(AVG(Outage_MW),2) AS Avg_Outage_MW_Loss, ROUND(AVG(ROUND((TIMESTAMPDIFF(MINUTE, Start_Time, End_Time)/60)/24,2)),2) AS Average_Outage_Duration_Time_Minutes
FROM AEMR
WHERE Reason = 'Forced' AND Status = 'Approved'
GROUP BY Participant_Code, Status, Year
ORDER BY Year, Average_Outage_Duration_Time_Minutes DESC;


SELECT Status, YEAR(Start_Time) AS Year, ROUND(AVG(Outage_MW),2) AS Avg_Outage_MW_Loss,
Cast(ROUND(AVG(Cast(TIMESTAMPDIFF(MINUTE, Start_Time, End_Time) AS DECIMAL(18,2))),2) AS DECIMAL(18,2)) AS Average_Outage_Duration_Time_Minutes
FROM AEMR
WHERE Reason = 'Forced' AND Status = 'Approved'
GROUP BY Status,Year
ORDER BY Year;


SELECT Status,Reason,YEAR(Start_Time) AS Year, ROUND(AVG(Outage_MW),2) AS Avg_Outage_MW_Loss,
Cast(ROUND(AVG(Cast(TIMESTAMPDIFF(MINUTE, Start_Time, End_Time) AS DECIMAL(18,2))),2) AS DECIMAL(18,2)) AS Average_Outage_Duration_Time_Minutes
FROM AEMR
WHERE Status = 'Approved'
GROUP BY Status,Reason,Year
ORDER BY Year;


Tableau Presentation Link;

https://public.tableau.com/profile/agogbua.ogochukwu.wilfred#!/

