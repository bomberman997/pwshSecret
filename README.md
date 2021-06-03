# pwshSecret
Encrypting your API keys in files when not in use! : HOWTO

--PROFILE

Your profile.ps1 file is a powerful tool for running commands upon launching the shell. Including decrypting your files!

Another great usecase was to have the shell location change to where I store my powershell scripts.

cd $HOME/code

dropped into profile imediatly took me to my folder holding all of my projects.


--MAKING ENCRYPTED FILE

"Hunter2" | ConvertTo-SecureString -AsPlainText -Force

This will take the string "Hunter2" and pipe it into the ConvertTo-SecureString 

-AsPlainText and -Force combined allow the string to be entered into a 'System.Security.SecureString' which is an object in powershell handling secure strings. 

Without these unsecured strings would not be allowed to be handled by a Secure Object

Essentially allowing the handling of raw input such as the plain text string "Hunter 2" we are starting with

Now we are going to combine this with ConvertFrom-SecureString cmdlet to convert this object to a secure string that can safely
be stored within a text file 

"Hunter2" | ConvertTo-SecureString -AsPlainText -Force | ConvertFrom-SecureString

Putting this into your shell will output our encrypted string

"01000000d08c9ddf0115d1118c7a00c04fc297eb01000000c3c3d5aff2db5d44910dda044d8a2f080000000002000000000010660000000100002000000027109742d1174056e73ef7258bdd52423c2eb7de05dbdba94ff78c1b27a41523000000000e80000000020000200000007fa5784e873aedd4556c89e2b8fc413410e6c98ad48e2a5fbff49288256b7214100000007f8173f9f3b90c1b9f3832ba1a66e2ea4000000074caef9fc261f51d06a545509d89b8897c9a94a75cf5aafebf7014bcd4ca46a1e19254d986a174ebc6b3c3831fcd5791b08be4694cb4dbd0caf02d117dc1c3c5"

Because Secure objects cannot be stored by powershell saving our secure string above to a file will allow us to call upon it later when its time to access our API using Out-File 

"Hunter2" | ConvertTo-SecureString -AsPlainText -Force | ConvertFrom-SecureString | Out-File "$home\code\Password.txt"

Now that we have our string securely in the file we now need code to load it from the file when were ready for it.

The kind of online service we are connecting too, Microsoft services like office365 and Azure will allow us to load credentials using Get-Credentials module

$user = "wbennet"
$File = "C:\Temp\Password.txt"
$pass = Get-Content $file | ConvertTo-SecureString
$creds = New-Object System.Management.Automation.PSCredential ($user, $pass)

Now we can login hands free and more secure then the classic text file!

Connect-MsolService -Credential $creds

Boom!