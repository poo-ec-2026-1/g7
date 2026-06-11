import javafx.application.Application;

import javafx.collections.FXCollections;

import javafx.collections.ObservableList;

import javafx.geometry.Insets;

import javafx.scene.Scene;

import javafx.scene.control.*;

import javafx.scene.layout.GridPane;

import javafx.scene.layout.HBox;

import javafx.scene.layout.VBox;

import javafx.stage.Stage;

public class LaboratorioApp extends Application {

    private ObservableList<ItemLaboratorio> listaItens = FXCollections.observableArrayList();
    private TableView<ItemLaboratorio> tableView = new TableView<>();

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Sistema de Gestão de Laboratório");

        // Layout principal
        VBox root = new VBox(10);
        root.setPadding(new Insets(10));

        // Formulário para adicionar itens
        GridPane formGrid = new GridPane();
        formGrid.setHgap(10);
        formGrid.setVgap(10);
        formGrid.setPadding(new Insets(10));

        Label lblTipoItem = new Label("Tipo de Item:");
        ComboBox<String> cbTipoItem = new ComboBox<>();
        cbTipoItem.getItems().addAll("Equipamento", "Amostra");
        cbTipoItem.setValue("Equipamento");

        Label lblIdentificador = new Label("Identificador:");
        TextField txtIdentificador = new TextField();
        txtIdentificador.setPromptText("Identificador");

        Label lblAtributo1 = new Label("Status/Tipo:");
        TextField txtAtributo1 = new TextField();
        txtAtributo1.setPromptText("Status (Equip.) / Tipo (Amostra)");

        Label lblAtributo2 = new Label("Localização/Data Coleta:");
        TextField txtAtributo2 = new TextField();
        txtAtributo2.setPromptText("Localização (Equip.) / Data Coleta (Amostra)");

        Button btnAdicionar = new Button("Adicionar Item");
        Button btnAtualizar = new Button("Atualizar Item Selecionado");
        Button btnExcluir = new Button("Excluir Item Selecionado");

        formGrid.add(lblTipoItem, 0, 0);
        formGrid.add(cbTipoItem, 1, 0);
        formGrid.add(lblIdentificador, 0, 1);
        formGrid.add(txtIdentificador, 1, 1);
        formGrid.add(lblAtributo1, 0, 2);
        formGrid.add(txtAtributo1, 1, 2);
        formGrid.add(lblAtributo2, 0, 3);
        formGrid.add(txtAtributo2, 1, 3);
        formGrid.add(new HBox(10, btnAdicionar, btnAtualizar, btnExcluir), 1, 4);

        // Configurar TableView
        TableColumn<ItemLaboratorio, String> colIdentificador = new TableColumn<>("Identificador");
        colIdentificador.setCellValueFactory(cellData -> new javafx.beans.property.SimpleStringProperty(cellData.getValue().getIdentificador()));

        TableColumn<ItemLaboratorio, String> colTipo = new TableColumn<>("Tipo");
        colTipo.setCellValueFactory(cellData -> {
            if (cellData.getValue() instanceof Equipamento) {
                return new javafx.beans.property.SimpleStringProperty("Equipamento");
            } else if (cellData.getValue() instanceof Amostra) {
                return new javafx.beans.property.SimpleStringProperty("Amostra");
            }
            return new javafx.beans.property.SimpleStringProperty("Desconhecido");
        });

        TableColumn<ItemLaboratorio, String> colDetalhe1 = new TableColumn<>("Detalhe 1");
        colDetalhe1.setCellValueFactory(cellData -> {
            if (cellData.getValue() instanceof Equipamento) {
                return new javafx.beans.property.SimpleStringProperty(((Equipamento) cellData.getValue()).getStatus());
            } else if (cellData.getValue() instanceof Amostra) {
                return new javafx.beans.property.SimpleStringProperty(((Amostra) cellData.getValue()).getTipo());
            }
            return new javafx.beans.property.SimpleStringProperty("");
        });

        TableColumn<ItemLaboratorio, String> colDetalhe2 = new TableColumn<>("Detalhe 2");
        colDetalhe2.setCellValueFactory(cellData -> {
            if (cellData.getValue() instanceof Equipamento) {
                return new javafx.beans.property.SimpleStringProperty(((Equipamento) cellData.getValue()).getLocalizacao());
            } else if (cellData.getValue() instanceof Amostra) {
                return new javafx.beans.property.SimpleStringProperty(((Amostra) cellData.getValue()).getDataColeta());
            }
            return new javafx.beans.property.SimpleStringProperty("");
        });

        tableView.getColumns().addAll(colIdentificador, colTipo, colDetalhe1, colDetalhe2);
        tableView.setItems(listaItens);

        // Ações dos botões
        btnAdicionar.setOnAction(e -> {
            String tipoItem = cbTipoItem.getValue();
            String identificador = txtIdentificador.getText();
            String atributo1 = txtAtributo1.getText();
            String atributo2 = txtAtributo2.getText();

            if (identificador.isEmpty() || atributo1.isEmpty()) {
                showAlert(Alert.AlertType.WARNING, "Campos Vazios", "Por favor, preencha todos os campos obrigatórios.");
                return;
            }

            if (tipoItem.equals("Equipamento")) {
                Equipamento novoEquipamento = new Equipamento(identificador, atributo1, atributo2);
                listaItens.add(novoEquipamento);
                novoEquipamento.cadastrar();
            } else if (tipoItem.equals("Amostra")) {
                Amostra novaAmostra = new Amostra(identificador, atributo1, atributo2);
                listaItens.add(novaAmostra);
                novaAmostra.cadastrar();
            }
            clearForm(txtIdentificador, txtAtributo1, txtAtributo2);
        });

        btnAtualizar.setOnAction(e -> {
            ItemLaboratorio selecionado = tableView.getSelectionModel().getSelectedItem();
            if (selecionado != null) {
                String novoValor = txtAtributo1.getText();
                if (novoValor.isEmpty()) {
                    showAlert(Alert.AlertType.WARNING, "Campo Vazio", "Por favor, insira um valor para atualização.");
                    return;
                }
                selecionado.atualizar(novoValor);
                tableView.refresh(); // Atualiza a exibição na tabela
                clearForm(txtIdentificador, txtAtributo1, txtAtributo2);
            } else {
                showAlert(Alert.AlertType.WARNING, "Nenhum Item Selecionado", "Por favor, selecione um item para atualizar.");
            }
        });

        btnExcluir.setOnAction(e -> {
            ItemLaboratorio selecionado = tableView.getSelectionModel().getSelectedItem();
            if (selecionado != null) {
                selecionado.excluir();
                listaItens.remove(selecionado);
            } else {
                showAlert(Alert.AlertType.WARNING, "Nenhum Item Selecionado", "Por favor, selecione um item para excluir.");
            }
        });

        root.getChildren().addAll(formGrid, tableView);

        Scene scene = new Scene(root, 800, 600);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    private void showAlert(Alert.AlertType type, String title, String message) {
        Alert alert = new Alert(type);
        alert.setTitle(title);
        alert.setHeaderText(null);
        alert.setContentText(message);
        alert.showAndWait();
    }

    private void clearForm(TextField txtIdentificador, TextField txtAtributo1, TextField txtAtributo2) {
        txtIdentificador.clear();
        txtAtributo1.clear();
        txtAtributo2.clear();
    }

