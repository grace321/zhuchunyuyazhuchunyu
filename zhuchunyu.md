# zhuchunyuyazhuchunyu
text
JMenu[] menus=new JMenu[]{
		new JMenu("文件"),
		new JMenu("分析"),
		new JMenu("帮助")
	};
	
	JMenuItem optionofmenu[][]=new JMenuItem[][]{{
		new JMenuItem("文件"),
		new JMenuItem("新建"),
		new JMenuItem("打开"),
		

六、完整源码
第一个功能：
import java.awt.*;
import java.awt.event.*;

public class Mingan1 
{
    public static void main(String args[])
   {
        Frame f1=new Frame("敏感词分析程序");
        f1.setBounds(500, 100, 300, 200);
        f1.setBackground(Color.lightGray);
        f1.setLayout(new GridLayout(3,1));
        Panel p1=new Panel();
        f1.add(p1);
        Panel p2=new Panel();
        f1.add(p2);
        Panel p3=new Panel();
        f1.add(p3);
        f1.setVisible(true);    
        
        Label l1=new Label("请点击输入含有敏感词的文本文件：");
        p3.add(l1);
        Button b1=new Button("确定");
        p3.add(b1);
        b1.addActionListener(new ActionListener()
        {
            public void actionPerformed(ActionEvent e)
            {
                TestJMenu jmFrame=new TestJMenu();
            }
        });
        Label l2=new Label("敏感词汇保存在sensitive.txt文件中");
        p1.add(l2);
        p2.add(new Label("请查看sensitive.txt文件"));        
    }
}



第二个功能：
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import javax.swing.*;
import java.io.File;
import java.io.FileInputStream;

public class TestJMenu extends JFrame implements ActionListener
{
    JTextArea jta=new JTextArea();
      TestJMenu()
      {    
        this.setSize(400,300);
        JMenuBar jmb=new JMenuBar();
        JMenu jmFile=new JMenu("文件");
        JMenuItem jmiNew=new JMenuItem("新建");
        JMenuItem jmiOpen=new JMenuItem("打开");
        jmiOpen.addActionListener(this);
        jmFile.add(jmiNew);
        jmFile.add(jmiOpen);
        
        JMenu jmFenxi=new JMenu("分析");
        JMenuItem jmiQue=new JMenuItem("确定");
        JMenuItem jmiQu=new JMenuItem("取消");
        jmFenxi.add(jmiQue);
        jmFenxi.add(jmiQu);
         
        jmiQue.addActionListener(new ActionListener()
         {
            public void actionPerformed(ActionEvent e)
            {
             String filterText = jta.getText();
                try
                 {
                     jta.setText(Fenx.getFilterText(filterText));
                  } 
                catch(Exception exx)
                {
                    exx.printStackTrace();
                 }
             }                      
         });
        JMenu jmHelp=new JMenu("帮助");
        jmb.add(jmFile);
        jmb.add(jmFenxi);
        jmb.add(jmHelp);
        this.setJMenuBar(jmb);
        this.getContentPane().add(jta);
        this.setVisible(true);
      }

      public void actionPerformed(ActionEvent e) 
      {
        JFileChooser jc=new JFileChooser();
        jc.showOpenDialog(this);
        try
        {
            File file=jc.getSelectedFile();
            FileInputStream fis=new FileInputStream(file);
            byte[] buf=new byte[10*1024];
            int len=fis.read(buf);
            jta.append(new String(buf,0,len));
            
         }
          catch(Exception ex)
          {
            ex.printStackTrace();
           }
       }
    
    public static void main(String[] args)
    {
        TestJMenu jmFrame=new TestJMenu();
        jmFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
     }    
}

                 
第三个功能
import java.io.BufferedReader;
import java.io.FileReader;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Fenx 
{    
    public static void main(String[] args) throws Exception 
    {        
        String filterText = new String();
        System.out.println(getFilterText(filterText));  
     }

      public static String getFilterText(String filterText) throws Exception 
      {
        List listWord = new ArrayList();      
        FileReader reader = new FileReader("d:/sensitive.txt");
        BufferedReader br = new BufferedReader(reader);

        String s = null;
        while ((s = br.readLine())!= null) 
        {
            listWord.add(s.trim());
        }
        br.close();
        reader.close(); 

        Matcher m = null;
        String str1=new String();    
        for (int i = 0; i < listWord.size(); i++) 
       {
        int num=0;
        Pattern p=Pattern.compile(listWord.get(i).toString(),Pattern.CASE_INSENSITIVE);
         
            StringBuffer sb = new StringBuffer();
            m = p.matcher(filterText);
            while (m.find()) 
            {
               m.appendReplacement(sb, "口");
               num++;          
            }
            str1='\n'+"敏感词 "+p.toString()+" 出现:"+num+"次";
            //System.out.println('\n'+"敏感词 "+p.toString()+" 出现:"+num+"次");
            m.appendTail(sb);
            filterText = sb.toString(); 
            filterText+=str1;
        }       
        return filterText;        
     }    
}
