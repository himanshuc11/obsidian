### Rainbow Table Attacks

Databases store passwords in hashed formats. When user logs in, we hash the password and compare the value stored in the database.

- The attacker steals the database of hashed passwords
- The attacker boots up the rainbow table, a list of pre-computed passwords based on common input passwords.
- Attacker looks up the password inside the rainbow table and if he gets a match, then he can get the plain text password from the rainbow table

Solution
Salt the password
