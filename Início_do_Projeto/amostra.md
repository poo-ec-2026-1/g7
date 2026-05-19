
public class Amostra {

    private String codigo;
    private String tipo;
    private String dataColeta;

    public Amostra(String codigo, String tipo, String dataColeta) {
        this.codigo = codigo;
        this.tipo = tipo;
        this.dataColeta = dataColeta;
    }

    public void cadastrar() {
        System.out.println("Amostra cadastrada: " + codigo);
    }

    public void atualizar(String novoTipo) {
        this.tipo = novoTipo;
    }

    public void excluir() {
        System.out.println("Amostra excluída: " + codigo);
    }

    // Getters e Setters
    public String getCodigo() { return codigo; }
    public void setCodigo(String codigo) { this.codigo = codigo; }

    public String getTipo() { return tipo; }
    public void setTipo(String tipo) { this.tipo = tipo; }

    public String getDataColeta() { return dataColeta; }
    public void setDataColeta(String dataColeta) { this.dataColeta = dataColeta; }
}
