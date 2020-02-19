1.  Cree dos archivos de texto similares (con una o dos líneas diferentes). Compárelos empleando diff.

    ```powershell
    diff -ReferenceObject(cat file1.txt) -DifferenceObject(cat file2.txt)

    InputObject        SideIndicator
    -----------        -------------
    Pato               =>
    Electrocardiograma =>
    Gato               <=
    Elefante           <=

    ```

2.  Qué ocurre si se ejecuta:

    ```powershell
    get-service | export-csv servicios.csv | out-file
    ```

    Por qué?

    La salida del comando anterior es:

    ```powershell
    out-file : No se puede procesar el argumento porque el valor del argumento "path" es NULL. Cambie el valor del argumento "path" a un valor no nulo.
    En línea: 1 Carácter: 42
    + get-service | export-csv servicios.csv | out-file
    +                                          ~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Out-File], PSArgumentNullException
    + FullyQualifiedErrorId : ArgumentNull,Microsoft.PowerShell.Commands.OutFileCommand
    ```

    Esto ya que _export-csv servicios.csv_ crea el archivo csv y tiene como salida null, por tanto _out-file_ es innecesario

3.  Cómo haría para crear un archivo delimitado por puntos y comas (;)? PISTA: Se emplea export-csv, pero con un parámetro adicional.

    ```powershell
    ps | epcsv -delimiter ";" example.csv
    ```

4.  Export-cliXML y Export-CSV modifican el sistema, porque pueden crear y sobrescribir archivos. Existe algún parámetro que evite la sobrescritura de un archivo existente? Existe algún parámetro que permita que el comando pregunte antes de sobrescribir un archivo?

    Para no sobreescribir el archivo si existe se puede usar el atributo _-NoClobber_

    Ejemplo:
    El siguiente comando no sobrescribira el resultado del punto 3

    ```powershell
    ps | epcsv example.csv -NoClobber
    ```

    Para pedir confirmacion se utiliza el parametro _-Confirm_

    Ejeplo: El siguiente comando pedira confirmacion

    ```powershell
    ps | epcsv -delimiter ";" example.csv -Confirm
    ```

    La salida seria:

    ```powershell
    ¿Está seguro de que desea realizar esta acción?
    Se está realizando la operación "Export-Csv" en el destino "example.csv".
    [S] Sí  [O] Sí a todo  [N] No  [T] No a todo  [U] Suspender  [?] Ayuda
    (el valor predeterminado es "S"):
    ```

5.  Windows emplea configuraciones regionales, lo que incluye el separador de listas. En Windows en inglés, el separador de listas es la coma (,). Cómo se le dice a Export-CSV que emplee el separador del sistema en lugar de la coma?

    El cmdlet _get-culture_ obtiene la información de configuración del sistema, como el lenguaje e infomacion de escritura.

    ```powershell
    ps | epcsv test2.csv -Delimiter ((get-culture).textInfo.listSeparator)
    ```

    Usando _cat_ podemos ver que el resultado usa ';' como el separador de listas del español

    ```powershell
    cat .\test2.csv

    #TYPE System.Diagnostics.Process
    "Name";"SI";"Handles";"VM";"WS";"PM";"NPM";"Path";"Company";"CPU";"FileVersion";"ProductVersion";"Description";"Product";"\*\*NounName";"BasePriority";"ExitCode";"HasExited";"ExitTime";"Handle";"SafeHandle";"HandleCount";"Id";"MachineName";"MainWindowHandle";"MainWindowTitle";"MainModule";"MaxWorkingSet";"MinWorkingSet";"Modules";"NonpagedSystemMemorySize";"NonpagedSystemMemorySize64";"PagedMemorySize";"PagedMemorySize64";"PagedSystemMemorySize";"PagedSystemMemorySize64";"PeakPagedMemorySize";"PeakPagedMemorySize64";"PeakWorkingSet";"PeakWorkingSet64";"PeakVirtualMemorySize";"PeakVirtualMemorySize64";"PriorityBoostEnabled";"PriorityClass";"PrivateMemorySize";"PrivateMemorySize64";"PrivilegedProcessorTime";"ProcessName";"ProcessorAffinity";"Responding";"SessionId";"StartInfo";"StartTime";"SynchronizingObject";"Threads";"TotalProcessorTime";"UserProcessorTime";"VirtualMemorySize";"VirtualMemorySize64";"EnableRaisingEvents";"StandardInput";"StandardOutput";"StandardError";"WorkingSet";"WorkingSet64";"Site";"Container"
    "AdobeUpdateService";"0";"201";"59052032";"8044544";"2969600";"13216";;;;;;;;"Process";"8";;;;;;"201";"4164";".";"0";"";;;;;"13216";"13216";"2969600";"2969600";"108016";"108016";"3190784";"3190784";"8937472";"8937472";"62984192";"62984192";;;"2969600";"2969600";;"AdobeUpdateService";;"True";"0";"System.Diagnostics.ProcessStartInfo";;;"System.Diagnostics.ProcessThreadCollection";;;"59052032";"59052032";"False";;;;"8044544";"8044544";;
    "AGMService";"0";"217";"63569920";"10051584";"3067904";"13248";;;;;;;;"Process";"8";;;;;;"217";"4156";".";"0";"";;;;;"13248";"13248";"3067904";"3067904";"123240";"123240";"3690496";"3690496";"11776000";"11776000";"95219712";"95219712";;;"3067904";"3067904";;"AGMService";;"True";"0";"System.Diagnostics.ProcessStartInfo";;;"System.Diagnostics.ProcessThreadCollection";;;"63569920";"63569920";"False";;;;"10051584";"10051584";;
    "AGSService";"0";"417";"67899392";"15659008";"3981312";"16488";;;;;;;;"Process";"8";;;;;;"417";"4140";".";"0";"";;;;;"16488";"16488";"3981312";"3981312";"147392";"147392";"5971968";"5971968";"22687744";"22687744";"127795200";"127795200";;;"3981312";"3981312";;"AGSService";;"True";"0";"System.Diagnostics.ProcessStartInfo";;;"System.Diagnostics.ProcessThreadCollection";;;
    ```

6.  Identifique un cmdlet que permita generar un número aleatorio.

    El cmdlet _get-random_ devuelve un numero entero pseudo aleatorio de 32 bits.

    ```powershel
    get-random
    ```

7.  Identifique un cmdlet que despliegue la fecha y hora actuales.

    El cmdlet _get-date_ devuelve un objeto que representa la fecha y hora actuales, este puede ser representado en varios formatos como Windows o UNIX.

    ```
    get-date
    ```

8.  Qué tipo de objeto produce el cmdlet de la pregunta 7?

    usando el cmdlet gm podemos obtener los elementos que componen el objeto y el tipo, como se ve a continuacion el objeto es parte del paquete System y su tipo es DateTime

    ```
    get-date | gm

    TypeName: System.DateTime
    ```

9.  Usando el cmdlet de la pregunta 7 y select-object, despliegue solamente el día de la semana, así:

    ```powershell
    DayOfWeek
    ---------
    Thursday
    ```

    Para obtner este resultado lo hacemos con la siguiente linea:

    ```powershell
    get-date | select -property "dayofweek"
    ```

10. Identifique un cmdlet que muestre información acerca de parches (hotfixes) instalados en el sistema

    El cmdlet que nos retorna una lista con los parches del sistema es:

    ```powershell
    get-hotfix
    ```

11. Empleando el cmdlet de la pregunta 10, muestre una lista de parches instalados. Luego extienda la expresión para ordenar la lista por fecha de instalación, y muestre en pantalla únicamente la fecha de instalación, el usuario que instaló el parche, y el ID del parche. Recuerde examinar los nombres de las propiedades.

    ```powershell
    get-hotfix | sort -property installedon | select -property installedon, installedby, hotfixid
    ```

    Resultado

    ```powershell
    InstalledOn               installedby         hotfixid
    -----------               -----------         --------
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4520390
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4516115
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4515530
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4524569
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4525419
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4521863
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4503308
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4497727
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4517245
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4508433
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4515383
    6/12/2019 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4509096
    16/01/2020 12:00:00 a. m. NT AUTHORITY\SYSTEM KB4528759
    3/02/2020 12:00:00 a. m.  NT AUTHORITY\SYSTEM KB4534132
    11/02/2020 12:00:00 a. m. NT AUTHORITY\SYSTEM KB4538674
    11/02/2020 12:00:00 a. m. NT AUTHORITY\SYSTEM KB4532693
    11/02/2020 12:00:00 a. m. NT AUTHORITY\SYSTEM KB4537759
    12/02/2020 12:00:00 a. m. NT AUTHORITY\SYSTEM KB4524244
    ```

12. Complemente la solución a la pregunta 11, para que el sistema ordene los resultados por la descripción del parche, e incluya en el listado la descripción, el ID del parche, y la fecha de instalación. Escriba los resultados a un archivo HTML.

    ```powershell
    get-hotFix | sort -property description | select -property hotfixid,description,installedon  | convertto-html | Out-File hotfix.html
    ```

    Resultado

    ```powershell
    cat .\hotfix.html
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"  "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <title>HTML TABLE</title>
    </head><body>
    <table>
    <colgroup><col/><col/><col/></colgroup>
    <tr><th>hotfixid</th><th>description</th><th>InstalledOn</th></tr>
    <tr><td>KB4521863</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4520390</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4538674</td><td>Security Update</td><td>11/02/2020 12:00:00 a. m.</td></tr>
    <tr><td>KB4524244</td><td>Security Update</td><td>12/02/2020 12:00:00 a. m.</td></tr>
    <tr><td>KB4528759</td><td>Security Update</td><td>16/01/2020 12:00:00 a. m.</td></tr>
    <tr><td>KB4525419</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4524569</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4537759</td><td>Security Update</td><td>11/02/2020 12:00:00 a. m.</td></tr>
    <tr><td>KB4503308</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4497727</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4516115</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4508433</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4515530</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4515383</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4509096</td><td>Security Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    <tr><td>KB4532693</td><td>Update</td><td>11/02/2020 12:00:00 a. m.</td></tr>
    <tr><td>KB4534132</td><td>Update</td><td>3/02/2020 12:00:00 a. m.</td></tr>
    <tr><td>KB4517245</td><td>Update</td><td>6/12/2019 12:00:00 a. m.</td></tr>
    </table>
    </body></html>
    ```

13. Muestre una lista de las 50 entradas más nuevas del log de eventos System. Ordene la lista de modo que las entradas más antiguas aparezcan primero; las entradas producidas al mismo tiempo deben ordenarse por número índice. Muestre el número índice, la hora y la fuente para cada entrada. Escriba esta información en un archivo de texto plano.

    ```powershell
    get-eventlog -newest 50 -logname system | sort -property timegenerated -descending | sort -property index | select -property index, timegenerated, source > punto13.txt
    ```

    Resultado

    ```powershell
    cat .\punto13.txt

    Index TimeGenerated             Source
    ----- -------------             ------
    33624 19/02/2020 12:34:37 p. m. Netwtw06
    33625 19/02/2020 12:34:37 p. m. Netwtw06
    33626 19/02/2020 12:34:37 p. m. Netwtw06
    33627 19/02/2020 12:34:37 p. m. Netwtw06
    33628 19/02/2020 12:34:37 p. m. Netwtw06
    33629 19/02/2020 12:34:37 p. m. Netwtw06
    33630 19/02/2020 12:34:37 p. m. Netwtw06
    33631 19/02/2020 12:34:37 p. m. Netwtw06
    33632 19/02/2020 12:34:37 p. m. Netwtw06
    33633 19/02/2020 12:34:37 p. m. Netwtw06
    33634 19/02/2020 12:34:37 p. m. Netwtw06
    33635 19/02/2020 12:34:37 p. m. Netwtw06
    33636 19/02/2020 12:34:37 p. m. Netwtw06
    33637 19/02/2020 12:34:37 p. m. Netwtw06
    33638 19/02/2020 12:34:37 p. m. Netwtw06
    33639 19/02/2020 12:34:37 p. m. Netwtw06
    33640 19/02/2020 12:34:38 p. m. Netwtw06
    33641 19/02/2020 12:34:45 p. m. Microsoft-Windows-DNS-Client
    33642 19/02/2020 12:34:58 p. m. Microsoft-Windows-Kernel-Power
    33643 19/02/2020 12:36:02 p. m. Netwtw06
    33644 19/02/2020 12:36:02 p. m. Netwtw06
    33645 19/02/2020 12:36:02 p. m. Netwtw06
    33646 19/02/2020 12:36:02 p. m. Netwtw06
    33647 19/02/2020 12:36:02 p. m. Netwtw06
    33648 19/02/2020 12:36:05 p. m. Netwtw06
    33649 19/02/2020 12:36:05 p. m. Netwtw06
    33650 19/02/2020 12:36:05 p. m. Netwtw06
    33651 19/02/2020 12:36:05 p. m. Netwtw06
    33652 19/02/2020 12:36:05 p. m. Microsoft-Windows-WLAN-AutoConfig
    33653 19/02/2020 12:36:05 p. m. Service Control Manager
    33654 19/02/2020 12:36:09 p. m. Netwtw06
    33655 19/02/2020 1:24:53 p. m.  Microsoft-Windows-Kernel-Power
    33656 19/02/2020 1:24:55 p. m.  Microsoft-Windows-Kernel-Power
    33657 19/02/2020 1:32:43 p. m.  Microsoft-Windows-Kernel-General
    33658 19/02/2020 1:32:43 p. m.  Microsoft-Windows-Kernel-Power
    33659 19/02/2020 1:32:44 p. m.  Netwtw06
    33660 19/02/2020 1:32:44 p. m.  Netwtw06
    33661 19/02/2020 1:32:45 p. m.  Microsoft-Windows-Power-Troubleshooter
    33662 19/02/2020 1:32:53 p. m.  Microsoft-Windows-NDIS
    33663 19/02/2020 1:32:53 p. m.  Netwtw06
    33664 19/02/2020 1:32:53 p. m.  Netwtw06
    33665 19/02/2020 1:32:53 p. m.  Netwtw06
    33666 19/02/2020 1:59:22 p. m.  Netwtw06
    33667 19/02/2020 1:59:25 p. m.  Netwtw06
    33668 19/02/2020 1:59:25 p. m.  Netwtw06
    33669 19/02/2020 1:59:25 p. m.  Netwtw06
    33670 19/02/2020 1:59:25 p. m.  Netwtw06
    33671 19/02/2020 1:59:25 p. m.  Microsoft-Windows-WLAN-AutoConfig
    33672 19/02/2020 1:59:26 p. m.  Service Control Manager
    33673 19/02/2020 1:59:29 p. m.  Netwtw06
    ```
