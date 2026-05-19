public abstract class ItemLaboratorio {

    protected String identificador;

    public ItemLaboratorio(String identificador) {
        this.identificador = identificador;
    }

    public ItemLaboratorio() {
    }

    public void cadastrar() {
        System.out.println("Item cadastrado: " + identificador);
    }

    public void excluir() {
        System.out.println("Item excluído: " + identificador);
    }

    // Método abstrato → cada classe implementa do seu jeito
    public abstract void atualizar(String valor);

    public String getIdentificador() {
        return identificador;
    }

    public void setIdentificador(String identificador) {
        this.identificador = identificador;
    }
}
  
 
