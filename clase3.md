1. Cree dos archivos de texto similares (con una o dos líneas diferentes). Compárelos empleando diff.

    ```powershell
    diff -ReferenceObject(cat file1.txt) -DifferenceObject(cat file2.txt)

    InputObject        SideIndicator
    -----------        -------------
    Pato               =>
    Electrocardiograma =>
    Gato               <=
    Elefante           <=

    ```

2. Qué ocurre si se ejecuta:

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

3. Cómo haría para crear un archivo delimitado por puntos y comas (;)? PISTA: Se emplea export-csv, pero con un parámetro adicional.

    ```powershell
    ps | epcsv -delimiter ";" example.csv
    ```

4. Export-cliXML y Export-CSV modifican el sistema, porque pueden crear y sobrescribir archivos. Existe algún parámetro que evite la sobrescritura de un archivo existente? Existe algún parámetro que permita que el comando pregunte antes de sobrescribir un archivo?

    Para no sobreescribir el archivo si existe se puede usar el atributo _NoClobber_

    Ejemplo:
    El siguiente comando no sobrescribira el resultado del punto 3

    ```powershell
    ps | epcsv example.csv -NoClobber
    ```
