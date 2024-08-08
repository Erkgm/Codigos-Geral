package cargas;

public class Carregamento {
    private int id;
    private String descricao;
    private String dataCarregamento;
    private String nomeMotorista;
    private String cnh;
    private String peso;

    // Construtor completo
    public Carregamento(int id, String descricao, String dataCarregamento, String nomeMotorista, String cnh, String peso) {
        this.id = id;
        this.descricao = descricao;
        this.dataCarregamento = dataCarregamento;
        this.nomeMotorista = nomeMotorista;
        this.cnh = cnh;
        this.peso = peso;
    }

    // Getters e Setters
    public int getId() { return id; }
    public void setId(int id) { this.id = id; }

    public String getDescricao() { return descricao; }
    public void setDescricao(String descricao) { this.descricao = descricao; }

    public String getDataCarregamento() { return dataCarregamento; }
    public void setDataCarregamento(String dataCarregamento) { this.dataCarregamento = dataCarregamento; }

    public String getNomeMotorista() { return nomeMotorista; }
    public void setNomeMotorista(String nomeMotorista) { this.nomeMotorista = nomeMotorista; }

    public String getCnh() { return cnh; }
    public void setCnh(String cnh) { this.cnh = cnh; }

    public String getPeso() { return peso; }
    public void setPeso(String peso) { this.peso = peso; }
}
----------------------------------------------------------------------------------
package cargas;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

class FormularioCarregamentoFrame extends JFrame {
    private JTextField nomeMotoristaField;
    private JTextField cnhField;
    private JTextField descricaoCargaField;
    private JTextField pesoCargaField;
    private JTextField dataCarregamentoField;
    private JButton salvarButton;
    private MenuFrame menuFrame;
    private Carregamento carregamento;

    public FormularioCarregamentoFrame(MenuFrame menuFrame, Carregamento carregamentoSelecionado) {
        this.menuFrame = menuFrame;
        this.carregamento = carregamentoSelecionado;

        setTitle(carregamentoSelecionado == null ? "Novo Carregamento" : "Editar Carregamento");
        setSize(400, 350);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints constraints = new GridBagConstraints();
        constraints.insets = new Insets(10, 10, 10, 10);

        JLabel nomeMotoristaLabel = new JLabel("Nome do Motorista:");
        nomeMotoristaField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 0;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(nomeMotoristaLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(nomeMotoristaField, constraints);

        JLabel cnhLabel = new JLabel("CNH:");
        cnhField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 1;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(cnhLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(cnhField, constraints);

        JLabel descricaoCargaLabel = new JLabel("Descrição da Carga:");
        descricaoCargaField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 2;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(descricaoCargaLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(descricaoCargaField, constraints);

        JLabel pesoCargaLabel = new JLabel("Peso da Carga:");
        pesoCargaField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 3;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(pesoCargaLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(pesoCargaField, constraints);

        JLabel dataCarregamentoLabel = new JLabel("Data do Carregamento:");
        dataCarregamentoField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 4;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(dataCarregamentoLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(dataCarregamentoField, constraints);

        salvarButton = new JButton(carregamentoSelecionado == null ? "Salvar Carregamento" : "Atualizar Carregamento");

        salvarButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String nomeMotorista = nomeMotoristaField.getText();
                String cnh = cnhField.getText();
                String descricaoCarga = descricaoCargaField.getText();
                String pesoCarga = pesoCargaField.getText();
                String dataCarregamento = dataCarregamentoField.getText();

                if (nomeMotorista.isEmpty() || cnh.isEmpty() || descricaoCarga.isEmpty() || pesoCarga.isEmpty() || dataCarregamento.isEmpty()) {
                    JOptionPane.showMessageDialog(FormularioCarregamentoFrame.this, "Preencha todos os dados antes de salvar.", "Erro", JOptionPane.ERROR_MESSAGE);
                    return;
                }

                try {
                    if (carregamento == null) {
                        carregamento = new Carregamento(menuFrame.getProximoId(), descricaoCarga, dataCarregamento, nomeMotorista, cnh, pesoCarga);
                        menuFrame.adicionarCarregamento(carregamento);
                    } else {
                        carregamento.setNomeMotorista(nomeMotorista);
                        carregamento.setCnh(cnh);
                        carregamento.setDescricao(descricaoCarga);
                        carregamento.setPeso(pesoCarga);
                        carregamento.setDataCarregamento(dataCarregamento);
                    }

                    JOptionPane.showMessageDialog(FormularioCarregamentoFrame.this, "Carregamento salvo com sucesso!", "Sucesso", JOptionPane.INFORMATION_MESSAGE);
                    dispose();
                } catch (Exception ex) {
                    JOptionPane.showMessageDialog(FormularioCarregamentoFrame.this, "Erro ao salvar carregamento: " + ex.getMessage(), "Erro", JOptionPane.ERROR_MESSAGE);
                }
            }
        });

        constraints.gridx = 0;
        constraints.gridy = 5;
        constraints.gridwidth = 2;
        constraints.anchor = GridBagConstraints.CENTER;
        panel.add(salvarButton, constraints);

        if (carregamentoSelecionado != null) {
            nomeMotoristaField.setText(carregamentoSelecionado.getNomeMotorista());
            cnhField.setText(carregamentoSelecionado.getCnh());
            descricaoCargaField.setText(carregamentoSelecionado.getDescricao());
            pesoCargaField.setText(carregamentoSelecionado.getPeso());
            dataCarregamentoField.setText(carregamentoSelecionado.getDataCarregamento());
        }

        add(panel);
    }
}
------------------------------------------------------------
package cargas;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class ListaCarregamentosFrame extends JFrame {
    private MenuFrame menuFrame;
    private JList<String> listaCarregamentos;
    private DefaultListModel<String> listModel;

    public ListaCarregamentosFrame(MenuFrame menuFrame) {
        this.menuFrame = menuFrame;

        setTitle("Lista de Carregamentos");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
        setLocationRelativeTo(null);

        listModel = new DefaultListModel<>();
        listaCarregamentos = new JList<>(listModel);
        listaCarregamentos.setSelectionMode(ListSelectionModel.SINGLE_SELECTION);

        JScrollPane scrollPane = new JScrollPane(listaCarregamentos);

        JButton editarButton = new JButton("Editar");
        editarButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                int selectedIndex = listaCarregamentos.getSelectedIndex();
                if (selectedIndex != -1) {
                    Carregamento carregamentoSelecionado = menuFrame.getCarregamentos().get(selectedIndex);
                    FormularioCarregamentoFrame formularioFrame = new FormularioCarregamentoFrame(menuFrame, carregamentoSelecionado);
                    formularioFrame.setVisible(true);
                    dispose();
                }
            }
        });

        JButton excluirButton = new JButton("Excluir");
        excluirButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                int selectedIndex = listaCarregamentos.getSelectedIndex();
                if (selectedIndex != -1) {
                    menuFrame.getCarregamentos().remove(selectedIndex);
                    listModel.remove(selectedIndex);
                }
            }
        });

        JPanel buttonPanel = new JPanel();
        buttonPanel.add(editarButton);
        buttonPanel.add(excluirButton);

        add(scrollPane, BorderLayout.CENTER);
        add(buttonPanel, BorderLayout.SOUTH);

        carregarCarregamentos();
    }

    private void carregarCarregamentos() {
        listModel.clear();
        for (Carregamento carregamento : menuFrame.getCarregamentos()) {
            listModel.addElement(carregamento.getDescricao());
        }
    }
}
-----------------------------------------------------------
package cargas;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class LoginFrame extends JFrame {
    private JTextField usernameField;
    private JPasswordField passwordField;
    private JButton loginButton;

    public LoginFrame() {
        setTitle("Login");
        setSize(300, 200);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints constraints = new GridBagConstraints();
        constraints.insets = new Insets(10, 10, 10, 10);

        JLabel usernameLabel = new JLabel("Usuário:");
        constraints.gridx = 0;
        constraints.gridy = 0;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(usernameLabel, constraints);

        usernameField = new JTextField(15);
        constraints.gridx = 1;
        constraints.gridy = 0;
        constraints.anchor = GridBagConstraints.WEST;
        constraints.fill = GridBagConstraints.HORIZONTAL;
        panel.add(usernameField, constraints);

        JLabel passwordLabel = new JLabel("Senha:");
        constraints.gridx = 0;
        constraints.gridy = 1;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(passwordLabel, constraints);

        passwordField = new JPasswordField(15);
        constraints.gridx = 1;
        constraints.gridy = 1;
        constraints.anchor = GridBagConstraints.WEST;
        constraints.fill = GridBagConstraints.HORIZONTAL;
        panel.add(passwordField, constraints);

        loginButton = new JButton("Login");
        constraints.gridx = 0;
        constraints.gridy = 2;
        constraints.gridwidth = 2;
        constraints.anchor = GridBagConstraints.CENTER;
        panel.add(loginButton, constraints);

        loginButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String username = usernameField.getText();
                String password = new String(passwordField.getPassword());

                if (!username.isEmpty() && !password.isEmpty()) {
                    JOptionPane.showMessageDialog(LoginFrame.this, "Login bem-sucedido!", "Sucesso", JOptionPane.INFORMATION_MESSAGE);
                    // Abrir a tela de menu após login bem-sucedido
                    MenuFrame menuFrame = new MenuFrame();
                    menuFrame.setVisible(true);
                    dispose(); // Fechar a tela de login
                } else {
                    JOptionPane.showMessageDialog(LoginFrame.this, "Por favor, preencha todos os campos.", "Erro", JOptionPane.ERROR_MESSAGE);
                }
            }
        });

        add(panel);
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new LoginFrame().setVisible(true);
            }
        });
    }
}
---------------------------------------------------------------------
package cargas;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;

public class MenuFrame extends JFrame {
    private JButton novoCarregamentoButton;
    private JButton acessarCarregamentosButton;
    private ArrayList<Carregamento> carregamentos;

    public MenuFrame() {
        carregamentos = new ArrayList<>();

        setTitle("Menu de Carregamentos");
        setSize(400, 300);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints constraints = new GridBagConstraints();
        constraints.insets = new Insets(10, 10, 10, 10);

        novoCarregamentoButton = new JButton("Novo Carregamento");
        novoCarregamentoButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                FormularioCarregamentoFrame formularioFrame = new FormularioCarregamentoFrame(MenuFrame.this, null);
                formularioFrame.setVisible(true);
            }
        });

        constraints.gridx = 0;
        constraints.gridy = 0;
        panel.add(novoCarregamentoButton, constraints);

        acessarCarregamentosButton = new JButton("Acessar Carregamentos");
        acessarCarregamentosButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                ListaCarregamentosFrame listagemFrame = new ListaCarregamentosFrame(MenuFrame.this);
                listagemFrame.setVisible(true);
            }
        });

        constraints.gridx = 0;
        constraints.gridy = 1;
        panel.add(acessarCarregamentosButton, constraints);

        add(panel);
    }

    public void adicionarCarregamento(Carregamento carregamento) {
        carregamentos.add(carregamento);
    }

    public ArrayList<Carregamento> getCarregamentos() {
        return carregamentos;
    }

    public int getProximoId() {
        return carregamentos.size() + 1;
    }
}
--------------------------------------------------------------------
package cargas;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class NovoCarregamento extends JFrame {
    private JTextField nomeMotoristaField;
    private JTextField cnhField;
    private JTextField descricaoField;
    private JTextField pesoField;
    private JTextField dataField;
    private JButton salvarButton;
    private MenuFrame menuFrame;

    public NovoCarregamento(MenuFrame menuFrame) {
        this.menuFrame = menuFrame;

        setTitle("Novo Carregamento");
        setSize(400, 350);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLocationRelativeTo(null);

        JPanel panel = new JPanel(new GridBagLayout());
        GridBagConstraints constraints = new GridBagConstraints();
        constraints.insets = new Insets(10, 10, 10, 10);

        JLabel nomeMotoristaLabel = new JLabel("Nome do Motorista:");
        nomeMotoristaField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 0;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(nomeMotoristaLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(nomeMotoristaField, constraints);

        JLabel cnhLabel = new JLabel("CNH:");
        cnhField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 1;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(cnhLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(cnhField, constraints);

        JLabel descricaoLabel = new JLabel("Descrição da Carga:");
        descricaoField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 2;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(descricaoLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(descricaoField, constraints);

        JLabel pesoLabel = new JLabel("Peso da Carga:");
        pesoField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 3;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(pesoLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(pesoField, constraints);

        JLabel dataLabel = new JLabel("Data do Carregamento:");
        dataField = new JTextField(15);
        constraints.gridx = 0;
        constraints.gridy = 4;
        constraints.anchor = GridBagConstraints.EAST;
        panel.add(dataLabel, constraints);

        constraints.gridx = 1;
        constraints.anchor = GridBagConstraints.WEST;
        panel.add(dataField, constraints);

        salvarButton = new JButton("Salvar Carregamento");

        salvarButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String nomeMotorista = nomeMotoristaField.getText();
                String cnh = cnhField.getText();
                String descricao = descricaoField.getText();
                String peso = pesoField.getText();
                String data = dataField.getText();

                if (nomeMotorista.isEmpty() || cnh.isEmpty() || descricao.isEmpty() || peso.isEmpty() || data.isEmpty()) {
                    JOptionPane.showMessageDialog(NovoCarregamento.this, "Preencha todos os dados antes de salvar.", "Erro", JOptionPane.ERROR_MESSAGE);
                    return;
                }

                try {
                    Carregamento novoCarregamento = new Carregamento(menuFrame.getProximoId(), descricao, data, nomeMotorista, cnh, peso);
                    menuFrame.adicionarCarregamento(novoCarregamento);
                    JOptionPane.showMessageDialog(NovoCarregamento.this, "Carregamento salvo com sucesso!", "Sucesso", JOptionPane.INFORMATION_MESSAGE);
                    dispose(); // Fechar a tela de novo carregamento
                } catch (Exception ex) {
                    JOptionPane.showMessageDialog(NovoCarregamento.this, "Erro ao salvar carregamento: " + ex.getMessage(), "Erro", JOptionPane.ERROR_MESSAGE);
                }
            }
        });

        constraints.gridx = 0;
        constraints.gridy = 5;
        constraints.gridwidth = 2;
        constraints.anchor = GridBagConstraints.CENTER;
        panel.add(salvarButton, constraints);

        add(panel);
    }
}
------------------------------------------------
module Carregamento {
    requires java.desktop;
}
-------------------------------
