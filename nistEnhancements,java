/*
 * SDEV 425 HW2
 * Modified UMUC homework
 * assignment. 
 */
package sdev425hw2;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.time.LocalTime;
import java.time.ZoneId;
import java.time.ZoneOffset;
import java.time.format.DateTimeFormatter;
import java.util.Date;
import java.util.Optional;
import javafx.application.Application;
import static javafx.application.Application.launch;
import javafx.application.Platform;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;
import javafx.scene.control.Button;
import javafx.scene.control.ButtonType;
import javafx.scene.control.Label;
import javafx.scene.control.PasswordField;
import javafx.scene.control.TextField;
import javafx.scene.layout.GridPane;
import javafx.scene.paint.Color;
import javafx.scene.text.Text;
import javafx.stage.Stage;

/**
 * @author Krystal Mordecai
 * 09/14/2018
 */
public class SDEV425HW2 extends Application {
    int attempts = 5; // Initializes login attempts to five.

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("SDEV425 Login");
        GridPane grid = new GridPane();
        grid.setAlignment(Pos.CENTER);
        grid.setHgap(10);
        grid.setVgap(10);

        Text scenetitle = new Text("Welcome. Login to continue.");
        grid.add(scenetitle, 0, 0, 2, 1);

        Label userName = new Label("User Name:");
        grid.add(userName, 0, 1);

        TextField userTextField = new TextField();
        grid.add(userTextField, 1, 1);

        Label pw = new Label("Password:");
        grid.add(pw, 0, 2);

        PasswordField pwBox = new PasswordField();
        grid.add(pwBox, 1, 2);
        
        Label pn = new Label("PIN:");
        grid.add(pn, 0, 3);

        TextField pin = new TextField();
        grid.add(pin, 1, 3);

        Button btn = new Button("Login");
        grid.add(btn, 1, 4);

        final Text actiontarget = new Text();
        grid.add(actiontarget, 1, 6);
        
        // AC-8 SYSTEM USE NOTIFICATION  Lines 78-98
        Alert alert = new Alert(AlertType.CONFIRMATION);
        alert.setTitle("User Agreement");
        alert.setHeaderText
            ("1. Users are accessing a U.S. Government information system;\n" +
            "2. Information system usage may be monitored, recorded, and subject to audit;\n" +
            "3. Unauthorized use of the information system is prohibited and subject to criminal and civil penalties; and\n" +
            "4. Use of the information system indicates consent to monitoring and recording;\n" +
            "    b. Retains the notification message or banner on the screen until users acknowledge the usage conditions and take "
                + "explicit actions to log on to or further access the information system; and\n" +
            "    c. For publicly accessible systems:\n" +
            "1. Displays system use information [Assignment: organization-defined conditions], before granting further access;\n" +
            "2. Displays references, if any, to monitoring, recording, or auditing that are consistent with privacy accommodations "
                + "for such systems that generally prohibit those activities; and\n" +
            "3. Includes a description of the authorized uses of the system. ");
        alert.setContentText("Click Continue to Accept");

        Optional<ButtonType> result = alert.showAndWait();
        if (result.get() == ButtonType.OK){ // Continues to application if ok chosen.
            } else {
                Platform.exit(); // Exits the application if cancel chosen.
        }

        btn.setOnAction(new EventHandler<ActionEvent>() {

            @Override
            public void handle(ActionEvent e) {
                // IA-2 IDENTIFICATION AND AUTHENTICATION (ORGANIZATIONAL USERS) Line 104
                boolean isValid = authenticate(userTextField.getText(), pwBox.getText(), pin.getText());
                // AC-7 UNSUCCESSFUL LOGON ATTEMPTS Line 107
                if (isValid && attempts >= 1) { // Checks number of attempts available before executing.
                    grid.setVisible(false);
                    GridPane grid2 = new GridPane();
                    grid2.setAlignment(Pos.CENTER);
                    grid2.setHgap(10);
                    grid2.setVgap(10);
                    Text scenetitle = new Text("Welcome " + userTextField.getText() + "!");
                    grid2.add(scenetitle, 0, 0, 2, 1);
                    Scene scene = new Scene(grid2, 500, 400);
                    primaryStage.setScene(scene);
                    primaryStage.show();
                    try {
                        successfulLogin(); // Calls method to log successful login attempt. 
                        } catch (FileNotFoundException ex) {
                            System.out.println("File not found");
                            }
                        } else {  
                    // AC-7 UNSUCCESSFUL LOGON ATTEMPTS Lines 125-133
                    if (attempts >= 1 ) {
                        final Text actiontarget = new Text();
                        grid.add(actiontarget, 1, 6);
                        actiontarget.setFill(Color.FIREBRICK);
                        actiontarget.setText("Please try again.");
                        attempts--;} // Decresaes available attempts by 1.
                    else try {
                        unsuccessfulLogin(); // Calls method to log failed attempt.
                        attempts(); // Calls attempts method if insufficient attempts available. 
                    } catch (FileNotFoundException ex) {
                        try {
                            // AU-5 RESPONSE TO AUDIT PROCESSING FAILURES
                            alert(); // Calls alert method if unsuccessfulLogin() fails to execute.
                        } catch (FileNotFoundException ex1) {
                            Platform.exit();
                        }
                    }   
                }
            }
            // Disables login button when unsufficient attempts available.
             public void attempts(){
                btn.setDisable(true);
            }
        });
        Scene scene = new Scene(grid, 500, 400);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    
    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        launch(args);
    }

    /**
     * @param user the username entered
     * @param pword the password entered
     * @param auth the PIN entered
     * @return isValid true for authenticated
     */
    public boolean authenticate(String user, String pword, String auth) {
        boolean isValid = false;
        if (user.equalsIgnoreCase("sdevadmin")
                && pword.equals("425!pass")
                    && auth.equals("1234")) { // authenticates PIN number 1234
            isValid = true;
        }

        return isValid;
    }
    
       // AU-3 CONTENT OF AUDIT RECORDS Lines 182-214
        public static void unsuccessfulLogin() throws FileNotFoundException {
        Date date = new Date(); //Sends date to log file.
        
        // UTC organizational timestamp
        ZoneOffset zoneOffset = ZoneOffset.of("-08:00");
        ZoneId zoneId=ZoneId.ofOffset("UTC", zoneOffset);
        LocalTime offsetTime = LocalTime.now(zoneId);
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("hh:mm a");
        String formattedTime=offsetTime.format(formatter);
        
        PrintWriter pw = new PrintWriter(new File("Unsuccessful.csv")); //Writes date and timestamp to log file.
        StringBuilder sb = new StringBuilder();
        sb.append("ID"); 
        sb.append(',');
        sb.append("UserName"); 
        sb.append(',');
        sb.append("Source Time"); 
        sb.append(',');
        sb.append("Destination Time"); 
        sb.append('\n');

        sb.append("1"); 
        sb.append(',');
        sb.append("sdevadmin"); 
        sb.append(',');
        sb.append(date); 
        sb.append(',');
        sb.append(formattedTime); 
        sb.append('\n');

        pw.write(sb.toString());
        pw.close();
    }  
         
        // AU-3 CONTENT OF AUDIT RECORDS Lines 218-249
        public static void successfulLogin() throws FileNotFoundException {
        Date date = new Date(); //Sends date to log file.
        
        // UTC organizational timestamp
        ZoneOffset zoneOffset = ZoneOffset.of("-08:00");
        ZoneId zoneId=ZoneId.ofOffset("UTC", zoneOffset);
        LocalTime offsetTime = LocalTime.now(zoneId);
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("hh:mm a");
        String formattedTime=offsetTime.format(formatter);
        
        PrintWriter pw = new PrintWriter(new File("Successful.csv")); //Writes date and timestamp to log file.
        StringBuilder sb = new StringBuilder();
        sb.append("ID"); 
        sb.append(',');
        sb.append("UserName"); 
        sb.append(',');
        sb.append("Source Time"); 
        sb.append(',');
        sb.append("Destination Time"); 
        sb.append('\n');

        sb.append("1"); 
        sb.append(',');
        sb.append("sdevadmin"); 
        sb.append(',');
        sb.append(date); 
        sb.append(',');
        sb.append(formattedTime); 
        sb.append('\n');

        pw.write(sb.toString());
        pw.close();
    }  
        
        // AU-5 RESPONSE TO AUDIT PROCESSING FAILURES Lines 252-285
        public static void alert() throws FileNotFoundException {
        Date date = new Date(); //Sends date to log file.
        
        //AU-8 Time Stamps
        // UTC organizational timestamp
        ZoneOffset zoneOffset = ZoneOffset.of("-08:00");
        ZoneId zoneId=ZoneId.ofOffset("UTC", zoneOffset);
        LocalTime offsetTime = LocalTime.now(zoneId);
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("hh:mm a");
        String formattedTime=offsetTime.format(formatter);
        
        PrintWriter pw = new PrintWriter(new File("Successful.csv")); //Writes date and timestamp to log file.
        StringBuilder sb = new StringBuilder();
        sb.append("ID"); 
        sb.append(',');
        sb.append("UserName"); 
        sb.append(',');
        sb.append("Source Time"); 
        sb.append(',');
        sb.append("Destination Time"); 
        sb.append('\n');

        sb.append("1"); 
        sb.append(',');
        sb.append("sdevadmin"); 
        sb.append(',');
        sb.append(date); 
        sb.append(',');
        sb.append(formattedTime); 
        sb.append('\n');

        pw.write(sb.toString());
        pw.close();
    }  
}
