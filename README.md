import java.awt.*;
import java.awt.event.*;
import javax.swing.*;
import javax.swing.border.LineBorder;
import javax.swing.border.Border;
import java.util.*;

class Calculator{
    int framewidth = 360;
    int frameheight = 540;
    
    JFrame frame = new JFrame("Calculator");
    JLabel displaylabel = new JLabel();
    JPanel displaypanel = new JPanel();
    JLabel buttonlabel = new JLabel();
    JPanel buttonpanel = new JPanel();
    
    String A = "0";
    String operator = null;
    String B = null;
    
    //declaring array for storage of digits and operators
    String[] buttonvalues = {
        "AC", "^", "%", "÷", 
        "7", "8", "9", "×", 
        "4", "5", "6", "-",
        "1", "2", "3", "+",
        "0", ".", "<-", "="
    };
    String[] operatorsymbols = {"÷", "×", "-", "+", "=", "^", "%"};
    String[] clearsymbol = {"AC"};
    
    Calculator(){
        //working on JFrame
        frame.setVisible(true);
        frame.setSize(framewidth, frameheight);
        frame.setResizable(false);
        frame.setLocationRelativeTo(null);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new BorderLayout());
        
        //color pallette declaration
        Color disppandigitcolor = new Color(245,245,245);
        Color displaypanelcolor = new Color(86,86,86);
        Color digitcolor = new Color(100,149,237);
        Color digibgcolor = new Color(58,58,58);
        Color operatorbgcolor = new Color(2,62,138);
        Color backgroundcolor = new Color(44,44,44);
        Color clearsymcolor = new Color(0,168,107);
        Color error = new Color (238,89,33);
        
        //working of display label
        displaylabel.setBackground(displaypanelcolor);
        displaylabel.setForeground(disppandigitcolor);
        displaylabel.setFont(new Font("Arial", Font.PLAIN, 60));
        displaylabel.setHorizontalAlignment(JLabel.RIGHT);
        displaylabel.setOpaque(true);
        displaylabel.setText("0");
        
        //working on display panel
        displaypanel.setLayout(new BorderLayout());
        displaypanel.add(displaylabel);
        frame.add(displaypanel, BorderLayout.NORTH);
        
        //working on buttonpanel
        buttonpanel.setLayout(new GridLayout(5,4));
        buttonpanel.setBackground(backgroundcolor);
        frame.add(buttonpanel);
        
        for(int i = 0; i<buttonvalues.length; i++){
            JButton button = new JButton();
            String buttonvalue = buttonvalues[i];
            button.setFont(new Font("Arial", Font.PLAIN, 35));
            button.setText(buttonvalue);
            button.setFocusable(false);
            button.setBorder(new LineBorder(backgroundcolor,1,true));
            
            //setting colour of button panel
            if(Arrays.asList(operatorsymbols).contains(buttonvalue)){
                button.setBackground(operatorbgcolor);
                button.setForeground(digitcolor);
            }
            else if(Arrays.asList(clearsymbol).contains(buttonvalue)){
                button.setBackground(clearsymcolor);
                button.setForeground(digitcolor);
            }
            else{
                button.setBackground(digibgcolor);
                button.setForeground(digitcolor);
            }
            buttonpanel.add(button);
            
            button.addActionListener(new ActionListener(){
                public void actionPerformed(ActionEvent e){
                    JButton button = (JButton) e.getSource();
                    String buttonvalue = button.getText();
                    
                    //function of operators
                    if(Arrays.asList(operatorsymbols).contains(buttonvalue)){
                        
                        //function of percentage
                        if(buttonvalue == "%"){
                            double numdisplay = Double.parseDouble(displaylabel.getText());
                            numdisplay /= 100;
                            displaylabel.setText(removeZeroDecimal(numdisplay));
                        }
                        else if( buttonvalue == "="){
                            if (A != null){
                                B = displaylabel.getText();
                                double numA = Double.parseDouble(A);
                                double numB = Double.parseDouble(B);
                                
                                if (operator == "+"){
                                     displaylabel.setText(removeZeroDecimal(numA + numB));
                                 }
                                else if (operator == "-"){
                                     displaylabel.setText(removeZeroDecimal(numA - numB));
                                 }
                                else if (operator == "×"){
                                    displaylabel.setText(removeZeroDecimal(numA * numB));
                                 }  
                                else if(operator == "÷"){
                                    displaylabel.setText(removeZeroDecimal(numA/numB));
                                    if (numB == 0.0){
                                        displaylabel.setForeground(error);
                                        displaylabel.setFont(new Font("Arial", Font.BOLD, 35));
                                        displaylabel.setText("Can't divide by zero");
                                    }
                                }
                                else if(operator == "^"){
                                    displaylabel.setText(removeZeroDecimal(Math.pow(numA,numB)));
                                }
                            }
                        }
                        else if("+-×÷^".contains(buttonvalue)){
                            if(operator == null){
                                A = displaylabel.getText();
                                displaylabel.setText("0");
                                B = "0";
                            }
                            
                            operator = buttonvalue; //storing button value in operator variable
                        }
                    }
                    
                    //function of AC
                    else if (Arrays.asList(clearsymbol).contains(buttonvalue)){
                        if(buttonvalue == "AC"){
                            clearAll();
                            displaylabel.setText("0");
                        }
                    }
                    
                    //backspace and digits and decimal
                    else{
                        
                        //backspace
                        if (buttonvalue == "<-"){
                            if(displaylabel.getText()!="0"){
                                displaylabel.setText(displaylabel.getText().substring(0,displaylabel.getText().length()-1));
                                if(displaylabel.getText() == ""){
                                displaylabel.setText("0");
                                }
                            }
                        }
                        
                        //decimal
                        if(buttonvalue == "."){
                            if(displaylabel.getText().contains(buttonvalue)){
                                displaylabel.setText(buttonvalue);
                            }
                            else{
                                displaylabel.setText(displaylabel.getText()+buttonvalue);
                            }
                        }
                        
                        //digits
                        else if("0123456789".contains(buttonvalue)){
                            if(displaylabel.getText()=="0"){
                                displaylabel.setText(buttonvalue);
                            }
                            else{
                                displaylabel.setText(displaylabel.getText()+buttonvalue); 
                            }
                        }
                    }
                    
                }
                
            });
        
        }
        
    }
    
    //function of AC
    void clearAll(){
        A="0";
        operator = null;
        B = null;
    }
    
    //rem,oval of decimal for whole numbers
    String removeZeroDecimal(double numdisplay){
        if(numdisplay % 1 == 0){
            return Integer.toString((int)numdisplay);
        }
        return Double.toString(numdisplay);
    }
    
    public static void main(String args[]){
        Calculator calculator = new Calculator();
    }
}
