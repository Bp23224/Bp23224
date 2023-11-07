using Proyecto;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
class Program
{
    static List<Libro> libros = new List<Libro>();
    static List<Proyecto.Usuario> usuarios = new List<Usuario>();
    static string archivoLibros = "libros.txt";
    static string archivoUsuarios = "usuarios.txt";
    static void Main(string[] args)
    {
        CargarDatosDesdeArchivos();
        bool salir = false;
        while (!salir)
        {
            Console.Clear();
            Console.WriteLine("Biblioteca - Menú Principal");
            Console.WriteLine("1. Gestión de Libros");
            Console.WriteLine("2. Gestión de Usuarios");
            Console.WriteLine("3. Préstamo y Devolución");
            Console.WriteLine("4. Búsqueda y Visualización");
            Console.WriteLine("5. Estadísticas");
            Console.WriteLine("6. Salir");
            Console.Write("Seleccione una opción: ");
            int opcion = int.Parse(Console.ReadLine());
            switch (opcion)
            {
                case 1:
                    GestionarLibros();
                    break;
                case 2:
                    GestionarUsuarios();
                    break;
                case 3:
                    PrestamoYDevolucion();
                    break;
                case 4:
                    BuscarYVisualizar();
                    break;
                case 5:
                    GenerarEstadisticas();
                    break;
                case 6:
                    salir = true;
                    break;
                default:
                    Console.WriteLine("Opción no válida. Intente nuevamente.");
                    break;
            }
        }
        GuardarDatosEnArchivos();
    }
    static void GestionarLibros()
    {
        bool salir = false;
        while (!salir)
        {
            Console.Clear();
            Console.WriteLine("Gestión de Libros");
            Console.WriteLine("1. Agregar Libro");
            Console.WriteLine("2. Eliminar Libro");
            Console.WriteLine("3. Actualizar Libro");
            Console.WriteLine("4. Volver al Menú Principal");
            Console.Write("Seleccione una opción: ");
            int opcion = int.Parse(Console.ReadLine());
            switch (opcion)
            {
                case 1:
                    AgregarLibro();
                    break;
                case 2:
                    EliminarLibro();
                    break;
                case 3:
                    ActualizarLibro();
                    break;
                case 4:
                    salir = true;
                    break;
                default:
                    Console.WriteLine("Opción no válida. Intente nuevamente.");
                    break;
            }
        }
    }
    private static void ActualizarLibro()
    {
        throw new NotImplementedException();
    }
    private static void EliminarLibro()
    {
        throw new NotImplementedException();
    }
    private static void AgregarLibro()
    {
        throw new NotImplementedException();
    }
    static void GestionarUsuarios()
    {
        bool salir = false;
        while (!salir)
        {
            Console.WriteLine("Gestión de Usuarios");
            Console.WriteLine("1. Registrar Usuario");
            Console.WriteLine("2. Volver al Menú Principal");
            Console.Write("Seleccione una opción: ");
            int opcion = int.Parse(Console.ReadLine());
            switch (opcion)
            {
                case 1:
                    RegistrarUsuario();
                    break;
                case 2:
                    salir = true;
                    break;
                default:
                    Console.WriteLine("Opción no válida. Intente nuevamente.");
                    break;
            }
        }
    }
    static void RegistrarUsuario()
    {
        Console.Write("Nombre del usuario: ");
        string nombre = Console.ReadLine();
        Console.Write("Número de identificación: ");
        int numeroId = int.Parse(Console.ReadLine());
        Usuario nuevoUsuario = new Usuario
        {
            Nombre = nombre,
            NumeroIdentificacion = numeroId
        };
        usuarios.Add(nuevoUsuario);
        Console.WriteLine("Usuario registrado exitosamente.");
    }
    static void PrestamoYDevolucion()
    {
        bool salir = false;
        while (!salir)
        {
            Console.WriteLine("Préstamo y Devolución");
            Console.WriteLine("1. Realizar Préstamo");
            Console.WriteLine("2. Realizar Devolución");
            Console.WriteLine("3. Volver al Menú Principal");
            Console.Write("Seleccione una opción: ");
            int opcion = int.Parse(Console.ReadLine());
            switch (opcion)
            {
                case 1:
                    RealizarPrestamo();
                    break;
                case 2:
                    RealizarDevolucion();
                    break;
                case 3:
                    salir = true;
                    break;
                default:
                    Console.WriteLine("Opción no válida. Intente nuevamente.");
                    break;
            }
        }
    }
    static void RealizarPrestamo()
    {
        Console.Write("Número de identificación del usuario: ");
        int numeroId = int.Parse(Console.ReadLine());
        Usuario usuario = usuarios.FirstOrDefault(u => u.NumeroIdentificacion == numeroId);
        if (usuario != null)
        {
            Console.WriteLine("Libros disponibles para préstamo:");
            MostrarLibrosDisponibles();
            Console.Write("Título del libro a prestar: ");
            string titulo = Console.ReadLine();
            Libro libro = libros.FirstOrDefault(l => l.Titulo == titulo);
            if (libro != null)
            {
                if (usuario.LibrosPrestados.Count < 3)
                {
                    if (libro.EjemplaresDisponibles > 0)
                    {
                        usuario.LibrosPrestados.Add(titulo);
                        libro.EjemplaresDisponibles--;
                        Console.WriteLine("Préstamo realizado con éxito.");
                    }
                    else
                    {
                        Console.WriteLine("El libro no tiene ejemplares disponibles.");
                    }
                }
                else
                {
                    Console.WriteLine("El usuario ha alcanzado el límite de libros prestados.");
                }
            }
            else
            {
                Console.WriteLine("Libro no encontrado.");
            }
        }
        else
        {
            Console.WriteLine("Usuario no encontrado.");
        }
    }
    static void RealizarDevolucion()
    {
        Console.Write("Número de identificación del usuario: ");
        int numeroId = int.Parse(Console.ReadLine());
        Usuario usuario = usuarios.FirstOrDefault(u => u.NumeroIdentificacion == numeroId);
        if (usuario != null)
        {
            Console.WriteLine("Libros prestados por el usuario:");
            MostrarLibrosPrestados(usuario);
            Console.Write("Título del libro a devolver: ");
            string titulo = Console.ReadLine();
            if (usuario.LibrosPrestados.Contains(titulo))
            {
                Libro libro = libros.FirstOrDefault(l => l.Titulo == titulo);
                usuario.LibrosPrestados.Remove(titulo);
                libro.EjemplaresDisponibles++;
                Console.WriteLine("Devolución realizada con éxito.");
            }
            else
            {
                Console.WriteLine("El usuario no tiene ese libro prestado.");
            }
        }
        else
        {
            Console.WriteLine("Usuario no encontrado.");
        }
    }
    static void BuscarYVisualizar()
    {
        Console.WriteLine("Búsqueda y Visualización");
        Console.WriteLine("1. Buscar Libro por Título");
        Console.WriteLine("2. Buscar Libro por Autor");
        Console.WriteLine("3. Buscar Libro por Género");
        Console.WriteLine("4. Mostrar Libros Disponibles");
        Console.WriteLine("5. Volver al Menú Principal");
        Console.Write("Seleccione una opción: ");
        int opcion = int.Parse(Console.ReadLine());
        switch (opcion)
        {
            case 1:
                BuscarLibroPorTitulo();
                break;
            case 2:
                BuscarLibroPorAutor();
                break;
            case 3:
                BuscarLibroPorGenero();
                break;
            case 4:
                MostrarLibrosDisponibles();
                break;
            case 5:
                break;
            default:
                Console.WriteLine("Opción no válida. Intente nuevamente.");
                break;
        }
    }
    static void BuscarLibroPorTitulo()
    {
        Console.Write("Ingrese el título del libro a buscar: ");
        string titulo = Console.ReadLine();
        Libro libro = libros.FirstOrDefault(l => l.Titulo == titulo);
        if (libro != null)
        {
            Console.WriteLine("Información del libro:");
            MostrarInformacionLibro(libro);
        }
        else
        {
            Console.WriteLine("Libro no encontrado.");
        }
    }
    static void BuscarLibroPorAutor()
    {
        Console.Write("Ingrese el nombre del autor a buscar: ");
        string autor = Console.ReadLine();
        List<Libro> librosAutor = libros.Where(l => l.Autor == autor).ToList();
        if (librosAutor.Count > 0)
        {
            Console.WriteLine("Libros encontrados del autor:");
            foreach (var libro in librosAutor)
            {
                MostrarInformacionLibro(libro);
            }
        }
        else
        {
            Console.WriteLine("Autor no encontrado.");
        }
    }
    static void BuscarLibroPorGenero()
    {
        Console.Write("Ingrese el género del libro a buscar: ");
        string genero = Console.ReadLine();
        List<Libro> librosGenero = libros.Where(l => l.Genero == genero).ToList();
        if (librosGenero.Count > 0)
        {
            Console.WriteLine("Libros encontrados en el género:");
            foreach (var libro in librosGenero)
            {
                MostrarInformacionLibro(libro);
            }
        }
        else
        {
            Console.WriteLine("Género no encontrado.");
        }
    }
    static void MostrarLibrosDisponibles()
    {
        Console.WriteLine("Libros disponibles:");
        foreach (var libro in libros)
        {
            if (libro.EjemplaresDisponibles > 0)
            {
                MostrarInformacionLibro(libro);
            }
        }
    }
    static void MostrarLibrosPrestados(Usuario usuario)
    {
        foreach (var libro in usuario.LibrosPrestados)
        {
            Libro libroPrestado = libros.FirstOrDefault(l => l.Titulo == libro);
            if (libroPrestado != null)
            {
                MostrarInformacionLibro(libroPrestado);
            }
        }
    }
    static void MostrarInformacionLibro(Libro libro)
    {
        Console.WriteLine($"Título: {libro.Titulo}");
        Console.WriteLine($"Autor: {libro.Autor}");
        Console.WriteLine($"Género: {libro.Genero}");
        Console.WriteLine($"Ejemplares Disponibles: {libro.EjemplaresDisponibles}");
        Console.WriteLine("----------");
    }
    static void GenerarEstadisticas()
    {
        Console.WriteLine("Estadísticas");
        Console.WriteLine("1. Libros más populares");
        Console.WriteLine("2. Usuarios con más préstamos");
        Console.WriteLine("3. Volver al Menú Principal");
        Console.Write("Seleccione una opción: ");
        int opcion = int.Parse(Console.ReadLine());
        switch (opcion)
        {
            case 1:
                LibrosMasPopulares();
                break;
            case 2:
                UsuariosConMasPrestamos();
                break;
            case 3:
                break;
            default:
                Console.WriteLine("Opción no válida. Intente nuevamente.");
                break;
        }
    }
    static void LibrosMasPopulares()
    {
        var librosPopulares = libros.OrderByDescending(l => l.EjemplaresDisponibles).Take(5).ToList();
        Console.WriteLine("Los libros más populares son:");
        foreach (var libro in librosPopulares)
        {
            Console.WriteLine($"{libro.Titulo} ({libro.EjemplaresDisponibles} ejemplares disponibles)");
        }
    }
    static void UsuariosConMasPrestamos()
    {
        var usuariosConMasPrestamos = usuarios.OrderByDescending(u => u.LibrosPrestados.Count).Take(5).ToList();
        Console.WriteLine("Los usuarios con más préstamos son:");
        foreach (var usuario in usuariosConMasPrestamos)
        {
            Console.WriteLine($"{usuario.Nombre} ({usuario.LibrosPrestados.Count} libros prestados)");
        }
    }
    static void CargarDatosDesdeArchivos()
    {
        if (File.Exists(archivoLibros))
        {
            string[] lineasLibros = File.ReadAllLines(archivoLibros);
            libros = lineasLibros.Select(linea =>
            {
                var datos = linea.Split(',');
                return new Libro
                {
                    Titulo = datos[0],
                    Autor = datos[1],
                    Genero = datos[2],
                    EjemplaresDisponibles = int.Parse(datos[3])
                };
            }).ToList();
        }
        if (File.Exists(archivoUsuarios))
        {
            string[] lineasUsuarios = File.ReadAllLines(archivoUsuarios);
            usuarios = lineasUsuarios.Select(linea =>
            {
                var datos = linea.Split(',');
                return new Usuario
                {
                    Nombre = datos[0],
                    NumeroIdentificacion = int.Parse(datos[1]),
                    LibrosPrestados = datos[2].Split(';').ToList()
                };
            }).ToList();
        }
    }
    static void GuardarDatosEnArchivos()
    {
        File.WriteAllLines(archivoLibros, libros.Select(libro =>
            $"{libro.Titulo},{libro.Autor},{libro.Genero},{libro.EjemplaresDisponibles}"));
        File.WriteAllLines(archivoUsuarios, usuarios.Select(usuario =>
            $"{usuario.Nombre},{usuario.NumeroIdentificacion},{string.Join(";", usuario.LibrosPrestados)}"));
    }
} 
using System;
using System.Collections.Generic;
using System.Linq;
namespace Proyecto
{
    internal class Libro
    {
        static List<Libro> libros = new List<Libro>();
        static string archivoLibros = "libros.txt";
        static string archivoUsuarios = "usuarios.txt";
        public string Titulo { get; set; }
        public string Autor { get; set; }
        public string Genero { get; set; }
        public int EjemplaresDisponibles { get; set; }
        static void AgregarLibro()
        {
            Console.Clear();
            Console.Write("Título del libro: ");
            string titulo = Console.ReadLine();
            Console.Write("Autor del libro: ");
            string autor = Console.ReadLine();
            Console.Write("Género del libro: ");
            string genero = Console.ReadLine();
            Console.Write("Número de ejemplares disponibles: ");
            int ejemplares = int.Parse(Console.ReadLine());
            Libro nuevoLibro = new Libro
            {
                Titulo = titulo,
                Autor = autor,
                Genero = genero,
                EjemplaresDisponibles = ejemplares
            };
            libros.Add(nuevoLibro);
            Console.WriteLine("Libro agregado exitosamente.");
        }
        static void EliminarLibro()
        {
            Console.Write("Título del libro a eliminar: ");
            string titulo = Console.ReadLine();
            Libro libroAEliminar = libros.FirstOrDefault(l => l.Titulo == titulo);
            if (libroAEliminar != null)
            {
                libros.Remove(libroAEliminar);
                Console.WriteLine("Libro eliminado exitosamente.");
            }
            else
            {
                Console.WriteLine("Libro no encontrado.");
            }
        }
        static void ActualizarLibro()
        {
            Console.Write("Título del libro a actualizar: ");
            string titulo = Console.ReadLine();
            Libro libroAActualizar = libros.FirstOrDefault(l => l.Titulo == titulo);
            if (libroAActualizar != null)
            {
                Console.Write("Nuevo título (deje en blanco para mantener el mismo): ");
                string nuevoTitulo = Console.ReadLine();
                if (!string.IsNullOrEmpty(nuevoTitulo))
                    libroAActualizar.Titulo = nuevoTitulo;
                Console.Write("Nuevo autor (deje en blanco para mantener el mismo): ");
                string nuevoAutor = Console.ReadLine();
                if (!string.IsNullOrEmpty(nuevoAutor))
                    libroAActualizar.Autor = nuevoAutor;
                Console.Write("Nuevo género (deje en blanco para mantener el mismo): ");
                string nuevoGenero = Console.ReadLine();
                if (!string.IsNullOrEmpty(nuevoGenero))
                    libroAActualizar.Genero = nuevoGenero;
                Console.Write("Nuevo número de ejemplares disponibles (deje en blanco para mantener el mismo): ");
                string nuevoEjemplaresStr = Console.ReadLine();
                if (!string.IsNullOrEmpty(nuevoEjemplaresStr))
                {
                    int nuevoEjemplares = int.Parse(nuevoEjemplaresStr);
                    libroAActualizar.EjemplaresDisponibles = nuevoEjemplares;
                }
                Console.WriteLine("Libro actualizado exitosamente.");
            }
            else
            {
                Console.WriteLine("Libro no encontrado.");
            }
        }
    }
} 
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
namespace Proyecto
{
    internal class Usuario
    {
            public string Nombre { get; set; }
            public int NumeroIdentificacion { get; set; }
            public List<string> LibrosPrestados { get; set; } = new List<string>();
    }
}

