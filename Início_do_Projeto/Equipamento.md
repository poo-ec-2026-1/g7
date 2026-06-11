
public class Equipamento extends ItemLaboratorio {

    private String status;
    private String localizacao;

    public Equipamento(String nome, String status, String localizacao) {
        super(nome);
        this.status = status;
        this.localizacao = localizacao;
    }

    public Equipamento() {
    }

    @Override
    public void atualizar(String novoStatus) {
        this.status = novoStatus;
        System.out.println("Equipamento atualizado: " + identificador);
    }

    // Getters e Setters
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }

    public String getLocalizacao() { return localizacao; }
    public void setLocalizacao(String localizacao) { this.localizacao = localizacao; }
}
}

