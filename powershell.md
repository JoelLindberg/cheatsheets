# Powershell

## Pattern matching (search tools)

Powershell equivalents to linux `<cmd> | grep <pattern>`.

Use Select-String (similar to findstr.exe or grep). \
Alias: `sls`

Examples: Find the SQL service name.

~~~powershell
> Get-Service | Out-String -Stream | sls SQL
~~~

~~~powershell
> Get-Service | sls -InputObject {$_.Name} -Pattern SQL
~~~

Source: https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/select-string?view=powershell-7.3