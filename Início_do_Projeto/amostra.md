public class Amostra extends ItemLaboratorio {

    private String tipo;
    private String dataColeta;

    public Amostra(String codigo, String tipo, String dataColeta) {
        super(codigo);
        this.tipo = tipo;
        this.dataColeta = dataColeta;
    }

    public Amostra() {
    }

    @Override
    public void atualizar(String novoTipo) {
        this.tipo = novoTipo;
        System.out.println("Amostra atualizada: " + identificador);
    }

    // Getters e Setters
    public String getTipo() { return tipo; }
    public void setTipo(String tipo) { this.tipo = tipo; }

    public String getDataColeta() { return dataColeta; }
    public void setDataColeta(String dataColeta) { this.dataColeta = dataColeta; }
}

