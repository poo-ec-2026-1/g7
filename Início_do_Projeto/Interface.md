import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.HBox;
import javafx.scene.layout.VBox;
import javafx.scene.text.Font;
import javafx.scene.text.FontWeight;
import javafx.stage.Stage;

public class LaboratorioApp extends Application {

    private ObservableList<ItemLaboratorio> listaItens = FXCollections.observableArrayList();
    private TableView<ItemLaboratorio> tableView = new TableView<>();
    private Stage primaryStage;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        this.primaryStage = primaryStage;
        showLoginScene();
    }

    /**
     * Interface de Login
     */
    private void showLoginScene() {
        primaryStage.setTitle("Login - Sistema de Gestão de Laboratório");

        VBox loginRoot = new VBox(20);
        loginRoot.setPadding(new Insets(50));
        loginRoot.setAlignment(Pos.CENTER);
        loginRoot.setStyle("-fx-background-color: #f4f4f4;");

        Label lblTitle = new Label("Acesso ao Sistema");
        lblTitle.setFont(Font.font("System", FontWeight.BOLD, 24));
        lblTitle.setStyle("-fx-text-fill: #003049;");

        GridPane grid = new GridPane();
        grid.setAlignment(Pos.CENTER);
        grid.setHgap(10);
        grid.setVgap(15);

        Label lblUser = new Label("Usuário:");
        TextField txtUser = new TextField();
        txtUser.setPromptText("Digite seu usuário");

        Label lblPass = new Label("Senha:");
        PasswordField txtPass = new PasswordField();
        txtPass.setPromptText("Digite sua senha");

        grid.add(lblUser, 0, 0);
        grid.add(txtUser, 1, 0);
        grid.add(lblPass, 0, 1);
        grid.add(txtPass, 1, 1);

        Button btnLogin = new Button("Entrar");
        btnLogin.setStyle("-fx-background-color: #003049; -fx-text-fill: white; -fx-font-weight: bold; -fx-padding: 10 20;");
        
        Label lblMessage = new Label();
        lblMessage.setStyle("-fx-text-fill: red;");

        btnLogin.setOnAction(e -> {
            String user = txtUser.getText();
            String pass = txtPass.getText();

            // Lógica de autenticação simples (Exemplo: admin / 1234)
            if (user.equals("Laboratório") && pass.equals("1234")) {
                showMainScene();
            } else {
                lblMessage.setText("Usuário ou senha incorretos!");
            }
        });

        loginRoot.getChildren().addAll(lblTitle, grid, btnLogin, lblMessage);

        Scene scene = new Scene(loginRoot, 400, 350);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    /**
     * Interface Principal (Dashboard)
     */
    private void showMainScene() {
        primaryStage.setTitle("Painel de Controle - Gestão de Laboratório");

        VBox root = new VBox(15);
        root.setPadding(new Insets(20));
        root.setStyle("-fx-background-color: white;");

        // Header com botão de Logout
        HBox header = new HBox();
        header.setAlignment(Pos.CENTER_LEFT);
        Label lblDashboard = new Label("Gestão de Itens");
        lblDashboard.setFont(Font.font("System", FontWeight.BOLD, 20));
        lblDashboard.setStyle("-fx-text-fill: #003049;");
        
        Button btnLogout = new Button("Sair");
        btnLogout.setOnAction(e -> showLoginScene());
        
        // Espaçador para empurrar o botão de sair para a direita
        javafx.scene.layout.Region spacer = new javafx.scene.layout.Region();
        javafx.scene.layout.HBox.setHgrow(spacer, javafx.scene.layout.Priority.ALWAYS);
        
        header.getChildren().addAll(lblDashboard, spacer, btnLogout);

        // Formulário para adicionar itens
        GridPane formGrid = new GridPane();
        formGrid.setHgap(15);
        formGrid.setVgap(10);
        formGrid.setPadding(new Insets(10));
        formGrid.setStyle("-fx-border-color: #ddd; -fx-border-width: 1; -fx-padding: 15;");

        Label lblTipoItem = new Label("Tipo de Item:");
        ComboBox<String> cbTipoItem = new ComboBox<>();
        cbTipoItem.getItems().addAll("Equipamento", "Amostra");
        cbTipoItem.setValue("Equipamento");

        Label lblIdentificador = new Label("ID:");
        TextField txtIdentificador = new TextField();

        Label lblAtributo1 = new Label("Status/Tipo:");
        TextField txtAtributo1 = new TextField();

        Label lblAtributo2 = new Label("Localização/Data:");
        TextField txtAtributo2 = new TextField();

        Button btnAdicionar = new Button("Cadastrar");
        Button btnAtualizar = new Button("Atualizar");
        Button btnExcluir = new Button("Remover");

        formGrid.add(lblTipoItem, 0, 0);
        formGrid.add(cbTipoItem, 1, 0);
        formGrid.add(lblIdentificador, 0, 1);
        formGrid.add(txtIdentificador, 1, 1);
        formGrid.add(lblAtributo1, 2, 0);
        formGrid.add(txtAtributo1, 3, 0);
        formGrid.add(lblAtributo2, 2, 1);
        formGrid.add(txtAtributo2, 3, 1);
        
        HBox actions = new HBox(10, btnAdicionar, btnAtualizar, btnExcluir);
        actions.setPadding(new Insets(10, 0, 0, 0));
        formGrid.add(actions, 1, 2, 3, 1);

        // Configurar TableView
        tableView = new TableView<>();
        TableColumn<ItemLaboratorio, String> colID = new TableColumn<>("ID");
        colID.setCellValueFactory(cellData -> new javafx.beans.property.SimpleStringProperty(cellData.getValue().getIdentificador()));

        TableColumn<ItemLaboratorio, String> colTipo = new TableColumn<>("Categoria");
        colTipo.setCellValueFactory(cellData -> {
            String tipo = (cellData.getValue() instanceof Equipamento) ? "Equipamento" : "Amostra";
            return new javafx.beans.property.SimpleStringProperty(tipo);
        });

        TableColumn<ItemLaboratorio, String> colD1 = new TableColumn<>("Status/Tipo Material");
        colD1.setCellValueFactory(cellData -> {
            if (cellData.getValue() instanceof Equipamento) {
                return new javafx.beans.property.SimpleStringProperty(((Equipamento) cellData.getValue()).getStatus());
            } else {
                return new javafx.beans.property.SimpleStringProperty(((Amostra) cellData.getValue()).getTipo());
            }
        });

        TableColumn<ItemLaboratorio, String> colD2 = new TableColumn<>("Localização/Data Coleta");
        colD2.setCellValueFactory(cellData -> {
            if (cellData.getValue() instanceof Equipamento) {
                return new javafx.beans.property.SimpleStringProperty(((Equipamento) cellData.getValue()).getLocalizacao());
            } else {
                return new javafx.beans.property.SimpleStringProperty(((Amostra) cellData.getValue()).getDataColeta());
            }
        });

        tableView.getColumns().addAll(colID, colTipo, colD1, colD2);
        tableView.setItems(listaItens);
        tableView.setColumnResizePolicy(TableView.CONSTRAINED_RESIZE_POLICY);

        // Eventos
        btnAdicionar.setOnAction(e -> {
            String id = txtIdentificador.getText();
            String a1 = txtAtributo1.getText();
            String a2 = txtAtributo2.getText();

            if (id.isEmpty() || a1.isEmpty()) return;

            if (cbTipoItem.getValue().equals("Equipamento")) {
                listaItens.add(new Equipamento(id, a1, a2));
            } else {
                listaItens.add(new Amostra(id, a1, a2));
            }
            txtIdentificador.clear(); txtAtributo1.clear(); txtAtributo2.clear();
        });

        btnExcluir.setOnAction(e -> {
            ItemLaboratorio sel = tableView.getSelectionModel().getSelectedItem();
            if (sel != null) listaItens.remove(sel);
        });

        btnAtualizar.setOnAction(e -> {
            ItemLaboratorio sel = tableView.getSelectionModel().getSelectedItem();
            if (sel != null && !txtAtributo1.getText().isEmpty()) {
                sel.atualizar(txtAtributo1.getText());
                tableView.refresh();
            }
        });

        root.getChildren().addAll(header, formGrid, tableView);

        Scene scene = new Scene(root, 900, 650);
        primaryStage.setScene(scene);
        primaryStage.show();
    }
}
