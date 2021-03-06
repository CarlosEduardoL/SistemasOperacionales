1. ¿Cuál clase puede emplearse para consultar la dirección IP de un adaptador
   de red? ¿Posee dicha clase algún método para liberar un préstamo de
   dirección (lease) DHCP?
   
   Para consultar la clase que nos permita conocer la direccion ip escibimos lo siguiente.

   ```powershell 
   gwmi -li | ? __class -like '*networkadapter*'
   ```
   Esto nos da como resultado el Win32_NetworkAdapter que contiene la informacion de los adaptadores de red.

   como podemos ver a continuacion con esta linea
   ```powershell
   gwmi Win32_NetworkAdapter | gm | ? {$_.membertype -like method"}
   ```
   dicha clase no posee método para liberar un préstamo de
   dirección (lease) DHCP sin embargo la clase Win32_NetworkAdapterConfiguration si lo tiene.

   ```powershell
   gwmi Win32_NetworkAdapterConfiguration | gm | ? {$_.membertype -like "method"}
   ```
2. Despliegue una lista de parches empleando WMI (Microsoft se refiere a los
   parches con el nombre **quick-fix engineering**). Es diferente el listado al
   que produce el cmdlet ``Get-Hotfix``?

   Busco el nombre completo de la clase con
   ```
   gwmi -li | ? __class -like "*QuickFixEngineering*"
   ```
   y listo todos los hotfixes con 
   ```powershell
   gwmi Win32_QuickFixEngineering
   ```
   usando el comando diff podemos ver que no hay ninguna diferencia enre la linea anterior y get-hotfix
   ```powershell
   diff -ReferenceObject (gwmi Win32_QuickFixEngineering) -DifferenceObject (Get-Hotfix)
   ```

3. Empleando WMI, muestre una lista de servicios, que incluya su status actual,
   su modalidad de inicio, y las cuentas que emplean para hacer login.
   ```
   gwmi win32_service | fl status, startmode, startname
   ```
4. Empleando cmdlets de CIM, liste todas las clases del namespace
   ``SecurityCenter2``, que tengan **product** como parte del nombre.
   ```powershell
   get-cimclass -names root\SecurityCenter2 | ? cimclassname -like '*product*'
   ```
5. Empleando cmdlets de CIM, y los resultados del ejercicio anterior, muestre
   los nombres de las aplicaciones antispyware instaladas en el sistema.
   También puede consultar si hay productos antivirus instalados en el sistema.
   
   Anti virus instalados
   ```powershell
   get-cimclass -names root\SecurityCenter2 -class AntiVirusProduct
   ```
   Anti Spyware instalado
   ```
   ```powershell
   get-cimclass -names root\SecurityCenter2 -class AntiSpywareProduct
   ```
   ```