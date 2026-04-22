# Doctorsystem

SELECT u.username, u.firstname AS 'First Name',
	   u.lastname AS 'Last Name', 
	   e.endpointname AS 'Application', 
	   a.name AS 'Account Name', 
	   a.customproperty4 AS 'Password_Expiration_Date', 
	   DATEDIFF(a.customproperty4, SYSDATE()) AS 'Days_Until_Expiry' 
FROM users u JOIN user_accounts ua ON u.USERKEY = ua.USERKEY 
JOIN accounts a ON ua.ACCOUNTKEY = a.ACCOUNTKEY 
JOIN endpoints e ON a.ENDPOINTKEY = e.ENDPOINTKEY 
JOIN securitysystems s ON e.SECURITYSYSTEMKEY = s.SYSTEMKEY 
JOIN policyrule p ON s.POLICYRULE = p.POLICYRULEKEY 
WHERE u.STATUSKEY = 1 
AND a.status IN ('Manually Provisioned', 1, 'Active') 
AND e.endpointname IN ('AWSRDSPOSTGRESDB') 
AND s.POLICYRULE IS NOT NULL 
AND s.POLICYRULE <> '' 
AND s.STATUS = 1 
AND e.status = 1 
AND (DATEDIFF(a.customproperty4, SYSDATE()) = 4);
