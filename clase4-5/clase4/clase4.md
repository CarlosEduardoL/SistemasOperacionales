1. Mostrar una tabla de procesos que incluya únicamente los nombres de los
   procesos, sus IDs, y si están respondiendo a Windows (la propiedad
   ``Responding`` muestra eso). Haga que la tabla tome el mínimo de espacio
   horizontal, pero no permita que la información se trunque.

    ```powershell
    ps | ft -Property name, id, responding -Wrap
    ```

2. Muestre una tabla de procesos que incluya los nombres de los procesos y sus
   IDs. También incluya columnas para uso de memoria virtual y física;
   exprese dichos valores en megabytes (MB).

   ```powershell
   ps | ft -Property name, id, @{n='vm [MB]';e={$_.vm/1MB -as [INT]}}, @{n='pm [MB]';e={$_.pm/1MB -as [INT]}}
   ```

3. Emplee ``Get-EventLog`` para mostrar una lista de los logs de eventos
   disponibles (revise la ayuda para encontrar el parámetro que le permitirá
   obtener dicha información). Formatee la salida como una tabla que incluya
   el nombre de despliegue del log y el período de retención. Los encabezados
   de columna deben ser NombreLog y Per-Retencion.

    ```powershell
    get-eventlog -list  | ft -property @{n='NombreLog';e={$_.log}}, @{n='Per-Retencion';e={$_.MinimumRetentionDays}}
    ```

4. Muestre una lista de servicios, de tal manera que aparezcan agrupados los
   servicios que están iniciados y los que están detenidos. Los que están
   iniciados deben aparecer primero.

   ```powershell
   gsv | sort -property status -descending | ft -groupby status
   ```

5. Mostrar una lista a cuatro columnas de todos los directorios que están en
   el raíz de la unidad ``C:``

    ```powershell
    ls 'C:/' | format-wide name -col 4
    ```

6. Cree una lista formateada de todos los archivos ``.exe`` del directorio
   ``C:\Windows``. Debe mostrarse el nombre, la información de versión, y el
   tamaño del archivo. La propiedad de tamaño se llama ``length`` en Powershell,
   pero para mayor claridad, la columna se debe llamar **Tamaño** en su listado.

    ```powershell
    ls 'C:\Windows/*.exe' | ft name, @{n='Tamano';e={$_.Length}}, versionInfo -Wrap
    ```

7. Importe el módulo ``NetAdapter`` (empleando el comando ``Import-Module
   NetAdapter``).
   Empleando el cmdlet ``Get-NetAdapter``, muestre una lista de adaptadores no
   virtuales (adaptadores cuya propiedad Virtual sea falsa. El valor lógico
   falso es representado por Powershell como ``$False``).

   ```powershell
   Get-NetAdapter | ? {$_.Virtual -eq $False} | fl
   ```

8. Importe el módulo ``DnsClient``. Empleando el cmdlet ``Get-DnsClientCache``,
   muestre una lista de los registros ``A`` y ``AAAA`` que estén en el caché.
   Sugerencia: Si el caché está vacío, visite algunos sitios web para poblarlo.

   ```powershell
   Get-DnsClientCache -type 'A', 'AAAA' | fl
   ```

9. Genere una lista de todos los archivos ``.exe`` del directorio
   ``C:\Windows\System32`` que tengan más de 5 MB.
   ```powershel
   ls 'C:\Windows\System32/*.exe' | ? {$_.length -gt 5MB} | fl
   ```

10. Muestre una lista de parches que sean actualizaciones de seguridad.
      ```powershell
      Get-HotFix | ? {$_.description -eq "Security Update"} | fl
      ```

11. Muestre una lista de parches que hayan sido instalados por el
    usuario ``Administrador``, que sean actualizaciones. Si no tiene ninguno,
    busque parches instalados por el usuario ``System``. Note que algunos parches
    no tienen valor en el campo ``Installed By``.
    ```powershell
      Get-HotFix | ? {$_.Description -like "*Update*" -and $_.InstalledBy -like "*System*"} | fl
      ```

12. Genere una lista de todos los procesos que estén corriendo con el nombre
    **Conhost** o **Svchost**.
    ```powershell
    ps | ? {$_.Name -eq 'conhost' -or $_.Name -eq 'svchost'} | fl
    ```
