# SmartCampusApp
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.util.ArrayList;

public class SmartCampusApp {

    // --- DATA CLASSES ---
    static class User {
        String username, password, fullName, role; // role = "STUDENT" or "ADMIN"

        public User(String username, String password, String fullName, String role) {
            this.username = username;
            this.password = password;
            this.fullName = fullName;
            this.role = role;
        }
    }

    static class Complaint {
        private int id;
        private String studentName, category, description, priority, status, date;

        public Complaint(int id, String studentName, String category, String description, String priority, String date) {
            this.id = id;
            this.studentName = studentName;
            this.category = category;
            this.description = description;
            this.priority = priority;
            this.status = "Pending";
            this.date = date;
        }

        public int getId() { return id; }
        public String getStudentName() { return studentName; }
        public String getCategory() { return category; }
        public String getDescription() { return description; }
        public String getPriority() { return priority; }
        public String getStatus() { return status; }
        public String getDate() { return date; }
        public void setStatus(String status) { this.status = status; }
    }

    static class ServiceRequest {
        private int id;
        private String studentName, service, description, status;

        public ServiceRequest(int id, String studentName, String service, String description) {
            this.id = id;
            this.studentName = studentName;
            this.service = service;
            this.description = description;
            this.status = "Pending";
        }

        public int getId() { return id; }
        public String getStudentName() { return studentName; }
        public String getService() { return service; }
        public String getDescription() { return description; }
        public String getStatus() { return status; }
        public void setStatus(String status) { this.status = status; }
    }

    // --- GLOBAL DATA & CONSTANTS ---
    static ArrayList<User> users = new ArrayList<>();
    static ArrayList<Complaint> complaints = new ArrayList<>();
    static ArrayList<ServiceRequest> serviceRequests = new ArrayList<>();
    static int complaintCounter = 1001, serviceCounter = 5001;

    static User currentUser = null; // tracks who is logged in right now

    static final Color PRIMARY = new Color(30, 60, 114);
    static final Color SECONDARY = new Color(43, 89, 195);
    static final Color LIGHT_BG = new Color(242, 245, 250);
    static final String[] COLUMNS = {"ID", "Student", "Category", "Description", "Priority", "Status", "Date"};

    public static void main(String[] args) {
        addSampleUsers();
        addSampleData();
        SwingUtilities.invokeLater(SmartCampusApp::showLoginPage);
    }

    static void addSampleUsers() {
        users.add(new User("student1", "1234", "Kishore", "STUDENT"));
        users.add(new User("student2", "1234", "Rahul", "STUDENT"));
        users.add(new User("student3", "1234", "Priya", "STUDENT"));
        users.add(new User("admin", "admin", "Administrator", "ADMIN"));
    }

    static void addSampleData() {
        complaints.add(new Complaint(complaintCounter++, "Kishore", "Wi-Fi", "Wi-Fi is down in Lab 2.", "High", "21-08-2026"));
        complaints.add(new Complaint(complaintCounter++, "Rahul", "Classroom", "Projector fault in Room 204.", "Medium", "21-08-2026"));
        complaints.add(new Complaint(complaintCounter++, "Priya", "Cleanliness", "Room needs cleaning.", "Low", "21-08-2026"));
        serviceRequests.add(new ServiceRequest(serviceCounter++, "Kishore", "ID Card Replacement", "Card is damaged."));
    }

    // --- LOGIN ---
    static void showLoginPage() {
        JFrame frame = createFrame("Smart Campus - Login", 500, 480);

        JPanel header = new JPanel(new GridLayout(2, 1));
        header.setBackground(PRIMARY);
        header.setPreferredSize(new Dimension(500, 100));

        JLabel title = new JLabel("SMART CAMPUS", SwingConstants.CENTER);
        title.setForeground(Color.WHITE);
        title.setFont(new Font("Arial", Font.BOLD, 26));

        JLabel subtitle = new JLabel("Complaint & Service Management", SwingConstants.CENTER);
        subtitle.setForeground(Color.WHITE);

        header.add(title);
        header.add(subtitle);

        JPanel loginPanel = new JPanel(new GridLayout(6, 1, 8, 8));
        loginPanel.setBorder(BorderFactory.createEmptyBorder(20, 40, 20, 40));

        JTextField userField = new JTextField();
        JPasswordField passField = new JPasswordField();
        JButton loginBtn = new JButton("LOGIN");
        loginBtn.setBackground(SECONDARY);
        loginBtn.setForeground(Color.WHITE);

        loginPanel.add(new JLabel("Username"));
        loginPanel.add(userField);
        loginPanel.add(new JLabel("Password"));
        loginPanel.add(passField);
        loginPanel.add(loginBtn);
        loginPanel.add(new JLabel("<html><center>Students: student1/1234, student2/1234, student3/1234<br>Admin: admin/admin</center></html>", SwingConstants.CENTER));

        loginBtn.addActionListener(e -> {
            String user = userField.getText().trim();
            String pass = new String(passField.getPassword());

            User matched = null;
            for (User u : users) {
                if (u.username.equals(user) && u.password.equals(pass)) {
                    matched = u;
                    break;
                }
            }

            if (matched == null) {
                JOptionPane.showMessageDialog(frame, "Invalid credentials!", "Login Failed", JOptionPane.ERROR_MESSAGE);
                return;
            }

            currentUser = matched;
            frame.dispose();

            if (currentUser.role.equals("STUDENT")) {
                showStudentDashboard();
            } else {
                showAdminDashboard();
            }
        });

        frame.add(header, BorderLayout.NORTH);
        frame.add(loginPanel, BorderLayout.CENTER);
        frame.setVisible(true);
    }

    // --- STUDENT DASHBOARD ---
    static void showStudentDashboard() {
        JFrame frame = createFrame("Smart Campus - Student Dashboard", 900, 550);

        JPanel header = createHeader("STUDENT DASHBOARD - " + currentUser.fullName, frame);
        JPanel menu = new JPanel(new GridLayout(1, 4, 10, 10));
        menu.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));

        JButton compBtn = new JButton("Register Complaint");
        JButton viewBtn = new JButton("My Complaints");
        JButton servBtn = new JButton("Service Request");
        JButton refreshBtn = new JButton("Refresh");

        menu.add(compBtn);
        menu.add(viewBtn);
        menu.add(servBtn);
        menu.add(refreshBtn);

        JLabel welcome = new JLabel("<html><center><h1>Welcome, " + currentUser.fullName + "</h1><p>Report campus issues easily.</p></center></html>", SwingConstants.CENTER);

        compBtn.addActionListener(e -> showComplaintForm(frame));
        viewBtn.addActionListener(e -> showMyComplaintsDialog(frame));
        servBtn.addActionListener(e -> showServiceForm(frame));
        refreshBtn.addActionListener(e -> JOptionPane.showMessageDialog(frame, "Dashboard Refreshed."));

        frame.add(header, BorderLayout.NORTH);
        frame.add(welcome, BorderLayout.CENTER);
        frame.add(menu, BorderLayout.SOUTH);
        frame.setVisible(true);
    }

    // --- ADMIN DASHBOARD ---
    static void showAdminDashboard() {
        JFrame frame = createFrame("Smart Campus - Admin Dashboard", 1000, 600);

        JPanel header = createHeader("ADMIN DASHBOARD", frame);
        DefaultTableModel model = new DefaultTableModel(COLUMNS, 0);
        JTable table = new JTable(model);
        table.setRowHeight(25);

        refreshTable(model, null);

        JPanel bottom = new JPanel(new FlowLayout());
        JButton refreshBtn = new JButton("Refresh");
        JButton pendingBtn = new JButton("Set Pending");
        JButton progressBtn = new JButton("In Progress");
        JButton resolvedBtn = new JButton("Resolved");
        JButton deleteBtn = new JButton("Delete");

        bottom.add(refreshBtn);
        bottom.add(pendingBtn);
        bottom.add(progressBtn);
        bottom.add(resolvedBtn);
        bottom.add(deleteBtn);

        refreshBtn.addActionListener(e -> refreshTable(model, null));
        pendingBtn.addActionListener(e -> updateStatus(table, model, "Pending"));
        progressBtn.addActionListener(e -> updateStatus(table, model, "In Progress"));
        resolvedBtn.addActionListener(e -> updateStatus(table, model, "Resolved"));

        deleteBtn.addActionListener(e -> {
            int row = table.getSelectedRow();
            if (row == -1) {
                JOptionPane.showMessageDialog(frame, "Select a complaint!");
                return;
            }
            int id = Integer.parseInt(model.getValueAt(row, 0).toString());
            if (JOptionPane.showConfirmDialog(frame, "Delete complaint " + id + "?", "Confirm", JOptionPane.YES_NO_OPTION) == JOptionPane.YES_OPTION) {
                complaints.removeIf(c -> c.getId() == id);
                refreshTable(model, null);
            }
        });

        frame.add(header, BorderLayout.NORTH);
        frame.add(new JScrollPane(table), BorderLayout.CENTER);
        frame.add(bottom, BorderLayout.SOUTH);
        frame.setVisible(true);
    }

    // --- FORMS & DIALOGS ---
    static void showComplaintForm(JFrame parent) {
        JDialog dialog = createDialog(parent, "Register Complaint", 500, 380);
        JPanel panel = new JPanel(new GridLayout(5, 2, 10, 10));
        panel.setBorder(BorderFactory.createEmptyBorder(15, 15, 15, 15));

        JComboBox<String> catBox = new JComboBox<>(new String[]{"Wi-Fi", "Classroom", "Cleanliness", "Electricity", "Water", "Other"});
        JTextArea descArea = new JTextArea();
        JComboBox<String> prioBox = new JComboBox<>(new String[]{"Low", "Medium", "High"});
        JButton submitBtn = new JButton("Submit");

        panel.add(new JLabel("Submitting as:")); panel.add(new JLabel(currentUser.fullName));
        panel.add(new JLabel("Category:")); panel.add(catBox);
        panel.add(new JLabel("Description:")); panel.add(new JScrollPane(descArea));
        panel.add(new JLabel("Priority:")); panel.add(prioBox);
        panel.add(new JLabel("")); panel.add(submitBtn);

        submitBtn.addActionListener(e -> {
            String desc = descArea.getText().trim();
            if (desc.isEmpty()) {
                JOptionPane.showMessageDialog(dialog, "Please enter a description!");
                return;
            }
            String cat = (String) catBox.getSelectedItem();
            String prio = (cat.equals("Electricity") || cat.equals("Water")) ? "High" : (String) prioBox.getSelectedItem();

            Complaint c = new Complaint(complaintCounter++, currentUser.fullName, cat, desc, prio, "21-08-2026");
            complaints.add(c);
            JOptionPane.showMessageDialog(dialog, "Submitted! ID: " + c.getId());
            dialog.dispose();
        });

        dialog.add(panel);
        dialog.setVisible(true);
    }

    static void showServiceForm(JFrame parent) {
        JDialog dialog = createDialog(parent, "Service Request", 450, 280);
        JPanel panel = new JPanel(new GridLayout(4, 2, 10, 10));
        panel.setBorder(BorderFactory.createEmptyBorder(15, 15, 15, 15));

        JComboBox<String> servBox = new JComboBox<>(new String[]{"ID Card Replacement", "Bonafide Certificate", "Transport Pass", "Other"});
        JTextArea descArea = new JTextArea();
        JButton submitBtn = new JButton("Submit");

        panel.add(new JLabel("Submitting as:")); panel.add(new JLabel(currentUser.fullName));
        panel.add(new JLabel("Service:")); panel.add(servBox);
        panel.add(new JLabel("Description:")); panel.add(new JScrollPane(descArea));
        panel.add(new JLabel("")); panel.add(submitBtn);

        submitBtn.addActionListener(e -> {
            String desc = descArea.getText().trim();
            if (desc.isEmpty()) {
                JOptionPane.showMessageDialog(dialog, "Please enter a description!");
                return;
            }
            ServiceRequest req = new ServiceRequest(serviceCounter++, currentUser.fullName, (String) servBox.getSelectedItem(), desc);
            serviceRequests.add(req);
            JOptionPane.showMessageDialog(dialog, "Request Sent! ID: " + req.getId());
            dialog.dispose();
        });

        dialog.add(panel);
        dialog.setVisible(true);
    }

    static void showMyComplaintsDialog(JFrame parent) {
        JDialog dialog = createDialog(parent, "My Complaints", 800, 400);
        DefaultTableModel model = new DefaultTableModel(COLUMNS, 0);
        JTable table = new JTable(model);
        table.setRowHeight(25);
        refreshTable(model, currentUser.fullName);

        dialog.add(new JScrollPane(table));
        dialog.setVisible(true);
    }

    // --- UTILITIES ---
    static JFrame createFrame(String title, int w, int h) {
        JFrame frame = new JFrame(title);
        frame.setSize(w, h);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLocationRelativeTo(null);
        return frame;
    }

    static JDialog createDialog(JFrame parent, String title, int w, int h) {
        JDialog dialog = new JDialog(parent, title, true);
        dialog.setSize(w, h);
        dialog.setLocationRelativeTo(parent);
        return dialog;
    }

    static JPanel createHeader(String titleText, JFrame frame) {
        JPanel header = new JPanel(new BorderLayout());
        header.setBackground(PRIMARY);
        header.setPreferredSize(new Dimension(1000, 60));

        JLabel title = new JLabel("  " + titleText);
        title.setForeground(Color.WHITE);
        title.setFont(new Font("Arial", Font.BOLD, 20));

        JButton logout = new JButton("Logout");
        logout.addActionListener(e -> {
            frame.dispose();
            currentUser = null;
            showLoginPage();
        });

        header.add(title, BorderLayout.WEST);
        header.add(logout, BorderLayout.EAST);
        return header;
    }

    static void refreshTable(DefaultTableModel model, String filterStudentName) {
        model.setRowCount(0);
        for (Complaint c : complaints) {
            if (filterStudentName != null && !c.getStudentName().equals(filterStudentName)) {
                continue;
            }
            model.addRow(new Object[]{c.getId(), c.getStudentName(), c.getCategory(), c.getDescription(), c.getPriority(), c.getStatus(), c.getDate()});
        }
    }

    static void updateStatus(JTable table, DefaultTableModel model, String status) {
        int row = table.getSelectedRow();
        if (row == -1) {
            JOptionPane.showMessageDialog(table, "Please select a complaint!");
            return;
        }
        int id = Integer.parseInt(model.getValueAt(row, 0).toString());
        for (Complaint c : complaints) {
            if (c.getId() == id) {
                c.setStatus(status);
                break;
            }
        }
        refreshTable(model, null);
    }
}
