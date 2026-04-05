---
layout: default
title: Level 4
order: 4
---

# [Level 4](https://try2hack.nl/levels/level4-kdnvxs.xhtml)

In this challenge's source code we come across this:

```
<!-- content -->
    <!--[if !IE]>-->
    <object classid="java:PasswdLevel4.class" type="application/x-java-applet" height="370" width="330">
        <!--<![endif]-->
        <object classid="clsid:8AD9C840-044E-11D1-B3E9-00805F499D93" codebase="http://java.sun.com/update/1.6.0/jinstall-6u10-windows-i586.cab" height="370" width="330"> 
            <param name="code" value="PasswdLevel4">
        </object> 
        <!--[if !IE]>-->
    </object>
    <!--<![endif]-->
<!-- end content -->
```

and this is probably the reason there's no visible content in this page. `PasswdLevel4.class` is a [Java applet](en.wikipedia.org/wiki/Java_applet), which has been discontinued just like Adobe Flash from [Level 2](https://frncscrlnd.github.io/writeups/try2hack/Level-2). Unfortunately, there are no browser java emulators available, so we'll need to decompile the `.class` file (which is a compiled `.java` file) with [CFR](https://www.benf.org/other/cfr/). After downloading the `.jar` file and the `.class` file at try2hack.nl/levels/PasswdLevel4.class we can decompile it like this:

```
java -jar cfr-0.152.jar PasswdLevel4.class
```

this will return

```
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.MalformedURLException;
import java.net.URL;

public class PasswdLevel4
extends Applet
implements ActionListener {
    private URL finalurl;
    String infile;
    String[] inuser = new String[22];
    int totno = 0;
    InputStream countConn = null;
    BufferedReader countData = null;
    URL inURL = null;
    TextField txtlogin = new TextField();
    Label label1 = new Label();
    Label label2 = new Label();
    Label label3 = new Label();
    TextField txtpass = new TextField();
    Label lblstatus = new Label();
    Button ButOk = new Button();
    Button ButReset = new Button();
    Label lbltitle = new Label();

    void ButOk_ActionPerformed(ActionEvent actionEvent) {
        boolean bl = false;
        int n = 1;
        while (n <= this.totno / 2) {
            if (this.txtlogin.getText().trim().toUpperCase().intern() == this.inuser[2 * (n - 1) + 2].trim().toUpperCase().intern() && this.txtpass.getText().trim().toUpperCase().intern() == this.inuser[2 * (n - 1) + 3].trim().toUpperCase().intern()) {
                this.lblstatus.setText("Login Success, Loading..");
                bl = true;
                String string = this.inuser[1].trim().intern();
                String string2 = this.getParameter("targetframe");
                if (string2 == null) {
                    string2 = "_self";
                }
                try {
                    this.finalurl = new URL(this.getCodeBase(), string);
                }
                catch (MalformedURLException malformedURLException) {
                    this.lblstatus.setText("Bad URL");
                }
                this.getAppletContext().showDocument(this.finalurl, string2);
            }
            ++n;
        }
        if (!bl) {
            this.lblstatus.setText("Invaild Login or Password");
        }
    }

    void ButReset_ActionPerformed(ActionEvent actionEvent) {
        ((TextComponent)this.txtlogin).setText("");
        ((TextComponent)this.txtpass).setText("");
    }

    public void actionPerformed(ActionEvent actionEvent) {
        Object object = actionEvent.getSource();
        if (object == this.ButOk) {
            this.ButOk_ActionPerformed(actionEvent);
            return;
        }
        if (object == this.ButReset) {
            this.ButReset_ActionPerformed(actionEvent);
        }
    }

    public void destroy() {
        this.ButOk.setEnabled(false);
        this.ButReset.setEnabled(false);
        this.txtlogin.setVisible(false);
        this.txtpass.setVisible(false);
    }

    public void inFile() {
        new StringBuffer();
        try {
            String string;
            this.countConn = this.inURL.openStream();
            this.countData = new BufferedReader(new InputStreamReader(this.countConn));
            while ((string = this.countData.readLine()) != null) {
                if (this.totno < 21) {
                    ++this.totno;
                    this.inuser[this.totno] = string;
                    string = "";
                    continue;
                }
                this.lblstatus.setText("Cannot Exceed 10 users, Applet fail start!");
                this.destroy();
            }
        }
        catch (IOException iOException) {
            this.getAppletContext().showStatus("IO Error:" + iOException.getMessage());
        }
        try {
            this.countConn.close();
            this.countData.close();
            return;
        }
        catch (IOException iOException) {
            this.getAppletContext().showStatus("IO Error:" + iOException.getMessage());
            return;
        }
    }

    public void init() {
        this.setLayout(null);
        this.setSize(361, 191);
        this.add(this.txtlogin);
        this.txtlogin.setBounds(156, 72, 132, 24);
        this.label1.setText("Please Enter Login Name & Password");
        this.label1.setAlignment(1);
        this.add(this.label1);
        this.label1.setFont(new Font("Dialog", 1, 12));
        this.label1.setBounds(41, 36, 280, 24);
        this.label2.setText("Login");
        this.add(this.label2);
        this.label2.setFont(new Font("Dialog", 1, 12));
        this.label2.setBounds(75, 72, 36, 24);
        this.label3.setText("Password");
        this.add(this.label3);
        this.add(this.txtpass);
        this.txtpass.setEchoChar('*');
        this.txtpass.setBounds(156, 108, 132, 24);
        this.lblstatus.setAlignment(1);
        this.label3.setFont(new Font("Dialog", 1, 12));
        this.label3.setBounds(75, 108, 57, 21);
        this.add(this.lblstatus);
        this.lblstatus.setFont(new Font("Dialog", 1, 12));
        this.lblstatus.setBounds(14, 132, 344, 24);
        this.ButOk.setLabel("OK");
        this.add(this.ButOk);
        this.ButOk.setFont(new Font("Dialog", 1, 12));
        this.ButOk.setBounds(105, 156, 59, 23);
        this.ButReset.setLabel("Reset");
        this.add(this.ButReset);
        this.ButReset.setFont(new Font("Dialog", 1, 12));
        this.ButReset.setBounds(204, 156, 59, 23);
        this.lbltitle.setAlignment(1);
        this.add(this.lbltitle);
        this.lbltitle.setFont(new Font("Dialog", 1, 12));
        this.lbltitle.setBounds(12, 14, 336, 24);
        String string = this.getParameter("title");
        this.lbltitle.setText(string);
        this.ButOk.addActionListener(this);
        this.ButReset.addActionListener(this);
        this.infile = new String("level4");
        try {
            this.inURL = new URL(this.getCodeBase(), this.infile);
        }
        catch (MalformedURLException malformedURLException) {
            this.getAppletContext().showStatus("Bad Counter URL:" + this.inURL);
        }
        this.inFile();
    }
}
```

the crucial code is 

```
this.infile = new String("level4");
this.inURL = new URL(this.getCodeBase(), this.infile);
this.inFile();
```

This means that the program reads a file named `level4` from the same directory. Let's try to download it: `www.try2hack.nl/levels/level4`

This returns

```
 level5-fdvbdf.xhtml
appletking
pieceofcake
```

Since there's no way to emulate the applet, we can not input these credentials. We can, however, go directly to the next level: https://www.try2hack.nl/levels/level5-fdvbdf.xhtml. 