using System;
using System.Collections.Generic;

// Clases
class Marca
{
    public int ID_Marca { get; set; }
    public string Nombre { get; set; }
    public string Pais_Origen { get; set; }
}

class Moto
{
    public int ID_Moto { get; set; }
    public string Nombre { get; set; }
    public string Modelo { get; set; }
    public Marca Marca { get; set; }
}

class Cliente
{
    public int ID_Cliente { get; set; }
    public string Nombre { get; set; }
}

class Repuesto
{
    public int ID_Repuesto { get; set; }
    public string Nombre { get; set; }
    public decimal Precio_Unitario { get; set; }
}

// Inventario de repuestos
class Inventario_Repuesto
{
    public Repuesto Repuesto { get; set; }
    public int Cantidad_Disponible { get; set; }
}

// Relación Factura - Repuesto
class Factura_Repuesto
{
    public Factura Factura { get; set; }
    public Repuesto Repuesto { get; set; }
    public int Cantidad { get; set; }
    public decimal Subtotal => Cantidad * Repuesto.Precio_Unitario;
}

class Factura
{
    public int ID_Factura { get; set; }
    public string Cod_Factura { get; set; }
    public Cliente Cliente { get; set; }
    public Moto Moto { get; set; }
    public Marca Marca { get; set; }
    public DateTime Fecha { get; set; }
    public List<Factura_Repuesto> FacturaRepuestos { get; set; } = new List<Factura_Repuesto>();

    public decimal Precio_Repuestos => CalcularSubtotal();
    public decimal IVA => Precio_Repuestos * 0.19m; // IVA del 19%
    public decimal Total => Precio_Repuestos + IVA;

    private decimal CalcularSubtotal()
    {
        decimal subtotal = 0;
        foreach (var item in FacturaRepuestos)
        {
            subtotal += item.Subtotal;
        }
        return subtotal;
    }
}

// Programa principal
class Program
{
    static void Main()
    {
        // Marcas
        List<Marca> marcas = new List<Marca>
        {
            new Marca { ID_Marca = 1, Nombre = "Honda", Pais_Origen = "Japón" },
            new Marca { ID_Marca = 2, Nombre = "Yamaha", Pais_Origen = "Japón" },
            new Marca { ID_Marca = 3, Nombre = "Suzuki", Pais_Origen = "Japón" },
            new Marca { ID_Marca = 4, Nombre = "Kawasaki", Pais_Origen = "Japón" },
            new Marca { ID_Marca = 5, Nombre = "Ducati", Pais_Origen = "Italia" }
        };

        // Motos
        List<Moto> motos = new List<Moto>
        {
            new Moto { ID_Moto = 1, Nombre = "CBR600", Modelo = "2023", Marca = marcas[0] },
            new Moto { ID_Moto = 2, Nombre = "R1", Modelo = "2020", Marca = marcas[1] },
            new Moto { ID_Moto = 3, Nombre = "GSX750", Modelo = "2022", Marca = marcas[2] },
            new Moto { ID_Moto = 4, Nombre = "Ninja 400", Modelo = "2019", Marca = marcas[3] },
            new Moto { ID_Moto = 5, Nombre = "Panigale V4", Modelo = "2024", Marca = marcas[4] }
        };

        // Clientes
        List<Cliente> clientes = new List<Cliente>
        {
            new Cliente { ID_Cliente = 52684, Nombre = "Juan Pérez" },
            new Cliente { ID_Cliente = 62185, Nombre = "María López" },
            new Cliente { ID_Cliente = 81236, Nombre = "Carlos Gómez" },
            new Cliente { ID_Cliente = 96325, Nombre = "Ana Torres" },
            new Cliente { ID_Cliente = 45687, Nombre = "Luis Ramírez" }
        };

        // Repuestos
        List<Repuesto> repuestos = new List<Repuesto>
        {
            new Repuesto { ID_Repuesto = 1, Nombre = "Filtro de Aire", Precio_Unitario = 30000 },
            new Repuesto { ID_Repuesto = 2, Nombre = "Pastillas de Freno", Precio_Unitario = 80000 },
            new Repuesto { ID_Repuesto = 3, Nombre = "Bujías", Precio_Unitario = 30000 },
            new Repuesto { ID_Repuesto = 4, Nombre = "Aceite Sintético", Precio_Unitario = 40000 },
            new Repuesto { ID_Repuesto = 5, Nombre = "Kit de Transmisión", Precio_Unitario = 250000 },
            new Repuesto { ID_Repuesto = 6, Nombre = "Amortiguadores", Precio_Unitario = 300000 }
        };

        // Inventario de Repuestos
        List<Inventario_Repuesto> inventario = new List<Inventario_Repuesto>
        {
            new Inventario_Repuesto { Repuesto = repuestos[0], Cantidad_Disponible = 10 },
            new Inventario_Repuesto { Repuesto = repuestos[1], Cantidad_Disponible = 15 },
            new Inventario_Repuesto { Repuesto = repuestos[2], Cantidad_Disponible = 20 },
            new Inventario_Repuesto { Repuesto = repuestos[3], Cantidad_Disponible = 12 },
            new Inventario_Repuesto { Repuesto = repuestos[4], Cantidad_Disponible = 8 },
            new Inventario_Repuesto { Repuesto = repuestos[5], Cantidad_Disponible = 5 }
        };

        // Facturas con repuestos
        List<Factura> facturas = new List<Factura>
        {
            new Factura {
                ID_Factura = 1, Cod_Factura = "FT001", Cliente = clientes[0], Moto = motos[0], Marca = marcas[0], Fecha = DateTime.Parse("25/02/2023"),
                FacturaRepuestos = new List<Factura_Repuesto>
                {
                    new Factura_Repuesto { Repuesto = repuestos[0], Cantidad = 1 },
                    new Factura_Repuesto { Repuesto = repuestos[3], Cantidad = 1 }
                }
            },
            new Factura {
                ID_Factura = 2, Cod_Factura = "FT002", Cliente = clientes[1], Moto = motos[1], Marca = marcas[1], Fecha = DateTime.Parse("10/08/2025"),
                FacturaRepuestos = new List<Factura_Repuesto>
                {
                    new Factura_Repuesto { Repuesto = repuestos[1], Cantidad = 1 },
                    new Factura_Repuesto { Repuesto = repuestos[4], Cantidad = 1 }
                }
            }
        };

        // Mostrar facturas
        foreach (var factura in facturas)
        {
            Console.WriteLine($"Factura: {factura.Cod_Factura}");
            Console.WriteLine($"Cliente: {factura.Cliente.Nombre}");
            Console.WriteLine($"Moto: {factura.Moto.Nombre} ({factura.Marca.Nombre})");
            Console.WriteLine($"Fecha: {factura.Fecha.ToShortDateString()}");

            Console.WriteLine("Repuestos:");
            foreach (var fr in factura.FacturaRepuestos)
            {
                Console.WriteLine($"- {fr.Repuesto.Nombre} (Cantidad: {fr.Cantidad}) - ${fr.Subtotal}");
            }

            Console.WriteLine($"Subtotal Repuestos: ${factura.Precio_Repuestos}");
            Console.WriteLine($"IVA: ${factura.IVA}");
            Console.WriteLine($"Total: ${factura.Total}");
            Console.WriteLine("----------------------------------------");
        }
    }
}
