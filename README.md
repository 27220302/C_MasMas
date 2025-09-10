# C_MasMas
namespace ClassLibrary1
{
    // Sistema operativo simple: gestiona procesos y memoria de manera básica
    public class Class1
    {
        private List<Proceso> procesos = new();
        private int memoriaTotal = 1024; // Memoria total simulada (MB)
        private int memoriaUsada = 0;

        public bool CrearProceso(string nombre, int memoriaRequerida)
        {
            if (memoriaUsada + memoriaRequerida > memoriaTotal)
                return false; // No hay suficiente memoria

            var proceso = new Proceso
            {
                Id = procesos.Count + 1,
                Nombre = nombre,
                Memoria = memoriaRequerida,
                Estado = EstadoProceso.Ejecutando
            };
            procesos.Add(proceso);
            memoriaUsada += memoriaRequerida;
            return true;
        }

        public bool FinalizarProceso(int id)
        {
            var proceso = procesos.FirstOrDefault(p => p.Id == id && p.Estado == EstadoProceso.Ejecutando);
            if (proceso == null)
                return false;

            proceso.Estado = EstadoProceso.Finalizado;
            memoriaUsada -= proceso.Memoria;
            return true;
        }

        public IEnumerable<Proceso> ListarProcesos()
        {
            return procesos.ToList();
        }

        public int MemoriaDisponible() => memoriaTotal - memoriaUsada;
    }

    public class Proceso
    {
        public int Id { get; set; }
        public string Nombre { get; set; } = string.Empty;
        public int Memoria { get; set; }
        public EstadoProceso Estado { get; set; }
    }

    public enum EstadoProceso
    {
        Ejecutando,
        Finalizado
    }
}
