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

    ````powershell
    out-file : No se puede procesar el argumento porque el valor del argumento "path" es NULL. Cambie el valor del argumento "path" a un valor no nulo.
    En línea: 1 Carácter: 42
    + get-service | export-csv servicios.csv | out-file
    +                                          ~~~~~~~~
        + CategoryInfo          : InvalidArgument: (:) [Out-File], PSArgumentNullException
        + FullyQualifiedErrorId : ArgumentNull,Microsoft.PowerShell.Commands.OutFileCommand```
    ````

    Esto ya que _export-csv servicios.csv_ crea el archivo csv y tiene como salida null, por tanto _out-file_ es innecesario
