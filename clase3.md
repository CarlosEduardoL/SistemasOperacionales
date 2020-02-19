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
    ps | epcsv -delimiter ";" example.csv -noclobber -Confirm
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

6.
