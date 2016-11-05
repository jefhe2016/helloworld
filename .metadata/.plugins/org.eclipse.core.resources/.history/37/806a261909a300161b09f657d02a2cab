//import org.apache.commons.io.FileUtils;
import org.apache.pdfbox.pdmodel.PDDocument;
import org.apache.pdfbox.pdfparser.PDFParser;
import org.apache.pdfbox.util.PDFTextStripper;

import java.awt.BorderLayout;
import java.awt.Graphics;
import java.awt.event.*;
import java.awt.*;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.UnsupportedEncodingException;
import java.lang.*;
import java.net.*;
import java.util.Properties;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

import javax.swing.DefaultDesktopManager;
import javax.swing.JButton;
import javax.swing.JDesktopPane;
import javax.swing.JFileChooser;
import javax.swing.JFrame;
import javax.swing.JInternalFrame;
import javax.swing.JLabel;
import javax.swing.JMenu;
import javax.swing.JMenuBar;
import javax.swing.JMenuItem;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JPopupMenu;
import javax.swing.JScrollPane;
import javax.swing.JTextArea;
import javax.swing.JTextField;
import javax.swing.event.InternalFrameAdapter;
import javax.swing.event.InternalFrameEvent;
import javax.swing.*;

public class bokongithub{
	public static void main(String args[]){
		BoKWindow mainwin=new BoKWindow();
	}
}//main program ends

class DictOperations{
	String meaning="";
	boolean match=false;
	String wordsdict[]=null;
	void dictmatch(String word,File dict){
		try{
			BufferedReader reader0=new BufferedReader(new InputStreamReader(new FileInputStream(dict),"UTF-8"));
			String linedict=null;//each line of dict
			match=false;//reset match when starting
			while((linedict=reader0.readLine())!=null && match==false){
				//seperate line to words
				String regexdict="[\\s\\d\\p{Punct}]+";
				wordsdict=linedict.split(regexdict);
				for(int idict=0;idict<wordsdict.length;idict++){
					//look up using word
					meaning="";//reset the meaning
					if (wordsdict[idict].toLowerCase().equals(word.toLowerCase())){//convert to low case and try to match the word	
						match=true;
						break;
					}
					//matching +s,+ed
					if (word.toLowerCase().endsWith("s") || word.toLowerCase().endsWith("ed"))
						if(word.toLowerCase().startsWith(wordsdict[idict].toLowerCase())){
							match=true;
							break;
						}
					//matching +ing
					if(wordsdict[idict].length()>3)
						if(word.toLowerCase().endsWith("ing") && word.toLowerCase().startsWith(wordsdict[idict].toLowerCase().substring(0, wordsdict[idict].length()-2))){
							match=true;
							break;
						}
				}
				if(match==true)
					break;				
			}
			if(match==false)
					System.out.print(word+" :NOT matched in"+dict.getName()+"\n");		
			else
				for(int idict=1;idict<wordsdict.length;idict++)
						meaning=meaning+wordsdict[idict];
			reader0.close();
		}catch(Exception e1){
			e1.printStackTrace();
			}
	}//end of dictmatch
	
	void addSelectedToDict(BoKInternalFrame internalFrame,File dict){
		FileOutputStream out;
		try{
			out= new FileOutputStream(dict,true);
			byte b[]=("\r\n"+internalFrame.textknow.getSelectedText()).getBytes();
			out.write(b);
			out.flush();
			out.close();
		}catch(Exception e1){
			e1.printStackTrace();
			}
	}
}//end of dict operations

class BoKInternalFrame extends JInternalFrame{//the class for each internal frame
	JTextArea textknow;//text area for internal frame	
	String selectWord;
	public BoKInternalFrame(String title){
		super(title,true,true,true,true);
		textknow=new JTextArea();//create the text area for each internal frame
		
		add(new JScrollPane(textknow),BorderLayout.CENTER);//add scroll panel
		setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);//set close style
		
		addInternalFrameListener(new InternalFrameAdapter(){
			public void internalFrameActivated(InternalFrameEvent e){
				setLayer(JDesktopPane.DRAG_LAYER);
			}
			public void internalFrameDeactivated(InternalFrameEvent e){
				setLayer(JDesktopPane.DEFAULT_LAYER);
			}
		});	
	     repaint();
	}
    public void paint(Graphics g) { 
        super.paint(g);
        /* 执行画图功能 */
     g.setColor(Color.red);
     g.drawLine(0,205,getWidth(),205); 
     repaint();
//    paintChildren(g);
 //   paintBorder(g);
    }
    
	public JTextArea getJTextArea(){
		return textknow;
	}
}//internal frame class ends here

class BoKWindow extends JFrame implements ActionListener{//class for main frame
	final File dictknown=new File("/Users/jefhe/documents/workspace/bok","dictknown.txt");
	final File dictall=new File("/Users/jefhe/documents/workspace/bok","dictall.txt");	
	final File dictnew=new File("/Users/jefhe/documents/workspace/bok","dictnew.txt");	
	
	JDesktopPane desk;//create the desktop for internal frames

	JMenuBar menubar;
	JMenu menu;
	JMenuItem itemNew;//create the menu on the desktop
	
	JPopupMenu menuMouse1,menuMouse2,menuMouse3;
	JMenuItem itemNewword1,itemOldword1,itemNewword2,itemOldword2,itemNewword3,itemOldword3;	
	String selectedWord1,selectedWord2,selectedWord3;
	
	private JButton txtbtn=new JButton("open a txt file");//create the txt button
	
	URL url;
	JTextField urltext,keytext;
	byte b[]=new byte[118];
	private JButton urlbtn=new JButton("open a url");//create the url button	
	private JButton pdfbtn=new JButton("open a pdf");//create the pdf button	
	private JButton keybtn=new JButton("match with key words");
	private JButton rfcbtn=new JButton("reflesh rfc-index");//create the "reflesh rfc-index" button
	private JButton IDbtn=new JButton("reflesh ID-index");//create the "reflesh ID-index" button	
	
	public BoKInternalFrame newBoKInternalFrame1;//internal frame for txt
	public BoKInternalFrame newBoKInternalFrame2;//internal frame for url
	public BoKInternalFrame newBoKInternalFrame3;//internal frame for pdf
	
	public String IDRfcFlag=null;
	public DictOperations BoKlookup=new DictOperations();
	

	
	BoKWindow(){
		
		desk= new JDesktopPane();//create the desktop for internal frames
		desk.setDesktopManager(new DefaultDesktopManager());
		add(desk,BorderLayout.CENTER);
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		menubar=new JMenuBar();
		menu= new JMenu("edit");
		itemNew=new JMenuItem("new");
		itemNew.addActionListener(this);
		menu.add(itemNew);
		menubar.add(menu);
		setJMenuBar(menubar);//create the menu and menubar
		
		setBounds(100,100,300,300);
		setVisible(true);
		
		Panel panel=new Panel();
		panel.add(txtbtn);
		txtbtn.addActionListener(this);
		panel.add(pdfbtn);
		pdfbtn.addActionListener(this);	
		panel.add(rfcbtn);
		rfcbtn.addActionListener(this);	
		panel.add(IDbtn);
		IDbtn.addActionListener(this);	
		
		urltext=new JTextField(20);		
		panel.add(new JLabel("input URL here:"));
		panel.add(urltext);
		panel.add(urlbtn);
		urlbtn.addActionListener(this);	
		
		keytext=new JTextField(20);		
		panel.add(new JLabel("input keywords here:"));
		panel.add(keytext);
		panel.add(keybtn);
		keybtn.addActionListener(this);	
		
		add(panel,BorderLayout.SOUTH);
//		panel.repaint();
		
		newBoKInternalFrame1=new BoKInternalFrame("knowbody no1:txt");
		            
		newBoKInternalFrame1.setBounds(100, 100, 300, 300);
		newBoKInternalFrame1.setVisible(true);
		desk.add(newBoKInternalFrame1,JDesktopPane.DRAG_LAYER);
		
		newBoKInternalFrame2=new BoKInternalFrame("knowbody no2:url");
		newBoKInternalFrame2.setBounds(200, 200, 300, 300);
		newBoKInternalFrame2.setVisible(true);
		desk.add(newBoKInternalFrame2,JDesktopPane.DRAG_LAYER);

		newBoKInternalFrame3=new BoKInternalFrame("knowbody no3:pdf");
		newBoKInternalFrame3.setBounds(200, 200, 300, 300);
		newBoKInternalFrame3.setVisible(true);
		desk.add(newBoKInternalFrame3,JDesktopPane.DRAG_LAYER);
		
		menuMouse1=new JPopupMenu();
		itemOldword1=new JMenuItem("Old Word");
		itemNewword1=new JMenuItem("New Word");	
		menuMouse1.add(itemOldword1);
		menuMouse1.add(itemNewword1);
		
		newBoKInternalFrame1.textknow.addMouseListener(new MouseAdapter(){
			public void mousePressed(MouseEvent e){
				if(e.getModifiers()==InputEvent.BUTTON3_MASK)
					{System.out.print("mouse !! \n");					
					menuMouse1.show(newBoKInternalFrame1.textknow,e.getX(),e.getY());}
				}
		});
		
		itemOldword1.addActionListener(this);
		itemNewword1.addActionListener(this);
		
		menuMouse2=new JPopupMenu();
		itemOldword2=new JMenuItem("Old Word");
		itemNewword2=new JMenuItem("New Word");	
		menuMouse2.add(itemOldword2);
		menuMouse2.add(itemNewword2);
		
		newBoKInternalFrame2.textknow.addMouseListener(new MouseAdapter(){
			public void mousePressed(MouseEvent e){
				if(e.getModifiers()==InputEvent.BUTTON3_MASK)
					menuMouse2.show(newBoKInternalFrame2.textknow,e.getX(),e.getY());
				}
		});
		
		itemOldword2.addActionListener(this);
		itemNewword2.addActionListener(this);

		menuMouse3=new JPopupMenu();
		itemOldword3=new JMenuItem("Old Word");
		itemNewword3=new JMenuItem("New Word");	
		menuMouse3.add(itemOldword3);
		menuMouse3.add(itemNewword3);
		
		newBoKInternalFrame3.textknow.addMouseListener(new MouseAdapter(){
			public void mousePressed(MouseEvent e){
				if(e.getModifiers()==InputEvent.BUTTON3_MASK)
					menuMouse3.show(newBoKInternalFrame3.textknow,e.getX(),e.getY());
				}
		});
		
		itemOldword3.addActionListener(this);
		itemNewword3.addActionListener(this);
	}
		
	
	public void actionPerformed(ActionEvent e){
		File dictknown=new File("D:/bok","dictknown.txt");
		
		if(e.getSource()==txtbtn){
			new FileReadThread(newBoKInternalFrame1).start();//start to read a txt file
		}		
		if(e.getSource()==urlbtn){
			new UrlReadThread(newBoKInternalFrame2).start();//start to read a url file
		}
		if(e.getSource()==pdfbtn){
			new PdfReadThread(newBoKInternalFrame3).start();//start to read a pdf file
		}
		
		if(e.getSource()==keybtn){
//			new KeyMatchThread(newBoKInternalFrame3).start();//start to refresh the rfc index file
		}
		if(e.getSource()==rfcbtn){
			IDRfcFlag="rfc";
			new RfcRefresh(newBoKInternalFrame1).start();//start to refresh the rfc index file
		}
		if(e.getSource()==IDbtn){
			IDRfcFlag="id";
			new RfcRefresh(newBoKInternalFrame1).start();//start to refresh the rfc index file
		}
		
		if(e.getSource()==itemOldword1){//add the selected word to dict known
			System.out.print("known word 1: "+"\n");			
			BoKlookup.addSelectedToDict(newBoKInternalFrame1,dictknown);
		}
		if(e.getSource()==itemNewword1){
			System.out.print("new word 1: "+"\n");	
		}
		if(e.getSource()==itemOldword2){//add the selected word to dict known
			System.out.print("known word 2: "+"\n");	
			BoKlookup.addSelectedToDict(newBoKInternalFrame2,dictknown);
		}
		if(e.getSource()==itemNewword2){
			System.out.print("new word 2: "+"\n");	
		}
		if(e.getSource()==itemOldword3){//add the selected word to dict known
			System.out.print("known word 3: "+"\n");	
			BoKlookup.addSelectedToDict(newBoKInternalFrame3,dictknown);
		}
		if(e.getSource()==itemNewword3){
			System.out.print("new word 3: "+"\n");	
		}
		
	if(e.getSource()==itemNew){
	JInternalFrame a[]=desk.getAllFrames();{
	for(int i=0;i<=a.length;i++)
		desk.setLayer(a[i],JDesktopPane.DEFAULT_LAYER);
	}
	
	}
	}
	
	String translateLines(String oneLine){//translate a line
		String regex="[\\s\\d\\p{Punct}]+";
		String words[]=oneLine.split(regex);
		for(int i=0;i<words.length;i++){
			if(words[i]!=null && words[i].length()>1 && Pattern.compile("^[A-Za-z]").matcher(words[i]).find()){
				BoKlookup.match=false;
				BoKlookup.meaning="";
				BoKlookup.dictmatch(words[i], dictknown);
				if(BoKlookup.match==false){
					BoKlookup.dictmatch(words[i], dictall);
					if(BoKlookup.match==true)//found in dict all
						oneLine=oneLine.replace(words[i],words[i]+"/*"+BoKlookup.meaning+"*/");
						else{
							try{
								FileOutputStream out=new FileOutputStream(dictnew,true);
								byte b[]=("\r\n"+words[i]).getBytes();
								out.write(b);
								out.flush();
								out.close();
							}catch(Exception e1){
								e1.printStackTrace();
								}
						}
					
				}
			}
		}
		return oneLine;
	}
	
	//file read thread
	class FileReadThread extends Thread{
		
		private BoKInternalFrame firstwin;
		public FileReadThread(BoKInternalFrame firstwin1){
			this.firstwin=firstwin1;
		}
		
		@Override
		public void run(){
			JFileChooser chooser=new JFileChooser("d:");			
			int option=chooser.showOpenDialog(firstwin);
			if (option==JFileChooser.APPROVE_OPTION){
				File selFile=chooser.getSelectedFile();
				FileInputStream inputstream1=null;
				try{
					inputstream1=new FileInputStream(selFile);
					BufferedReader reader=new BufferedReader(new InputStreamReader(inputstream1,"UTF-8"));
					String line=null;
					while((line=reader.readLine())!=null){
						firstwin.textknow.append(translateLines(line)+"\n");
						Thread.sleep(3);
				}
				reader.close();
				}catch(Exception e1){
					e1.printStackTrace();
				}
				JOptionPane.showMessageDialog(firstwin, "read finish");
			}	
		}		
	}

	//pdf file read thread
	class PdfReadThread extends Thread{
		private BoKInternalFrame firstwin;
		public PdfReadThread(BoKInternalFrame firstwin1){
			this.firstwin=firstwin1;
		}
		
		@Override
		public void run(){
			JFileChooser chooser=new JFileChooser("d:/");
			int option=chooser.showOpenDialog(firstwin);
			if (option==JFileChooser.APPROVE_OPTION){
				File selFile=chooser.getSelectedFile();
				String line=null;
				FileInputStream inputstream=null;
				PDDocument document=null;				
				try{					
					inputstream=new FileInputStream(selFile);
					PDFParser parser=new PDFParser(inputstream);
					parser.parse();
					document=parser.getPDDocument();
					PDFTextStripper stripper=new PDFTextStripper();
					line=stripper.getText(document);
					String arr[]=line.split("\r\n");					
					System.out.print("get a pdf doc");
					for (int j=0;j<arr.length;j++){//for each line
						// Seperate "line" into words
						firstwin.textknow.append(translateLines(arr[j])+"\n");
						Thread.sleep(3);
				}					
				}catch(Exception e1){
					e1.printStackTrace();
				}
				JOptionPane.showMessageDialog(firstwin, "read finish");
			}
		}		
	}	
	
	//URL file read thread
	class UrlReadThread extends Thread{
		private BoKInternalFrame s2win;
		public UrlReadThread(BoKInternalFrame s2ndwin){
			this.s2win=s2ndwin;
		}
		
		@Override
		public void run(){
			
				try{
					int n=-1;
					s2win.textknow.setText(null);
					url=new URL(urltext.getText().trim());					
					System.out.println("get url -->"+url);					
//					Properties prop=System.getProperties();//set proxy
//					prop.setProperty("http.proxyHost", "proxy.huawei.com");
//					prop.setProperty("http.proxyPort", "8080");						
					InputStream in=url.openStream();
					BufferedReader reader=new BufferedReader(new InputStreamReader(in));
					String line=null;
					while ((line=reader.readLine())!=null){
						if(urltext.getText().endsWith(".txt")){
								s2win.textknow.append(translateLines(line)+"\n");
//								Thread.sleep(3);						
						}
					}
				}catch(MalformedURLException e1){
						urltext.setText(""+e1);
						return;
						}
					catch(IOException e1){
						urltext.setText(""+e1);
						return;
						}
				JOptionPane.showMessageDialog(s2win, "read finish");						
				}
		}

	class RfcRefresh extends Thread{	
		private BoKInternalFrame firstwin;
		File RfcIndexLocal=new File("/Users/jefhe/documents/workspace/bok","RfcIndexLocal.txt");
		File IDIndexLocal=new File("/Users/jefhe/documents/workspace/bok","IDIndexLocal.txt");		
		String latestLocalRfc="4827",latestLocalID="draft-ymbk-sidr-transfer-01";
	
		public RfcRefresh(BoKInternalFrame firstwin1){
			this.firstwin=firstwin1;
		}
	
		public void writeOneLine(String oneLine,File targetFile){
			try{
				FileOutputStream out=new FileOutputStream(targetFile,true);
				byte b2[]=(oneLine+"\r\n").getBytes();
				out.write(b2);
				out.flush();
				out.close();
			}catch(Exception e1){
				e1.printStackTrace();
			}
		}
		@Override
		public void run(){		
		try{
			int IndexCounter=0;
			URL urlRFC=new URL("http://www.ietf.org/download/rfc-index.txt"); //input url of "online rfc index" file first
			URL urlID=new URL("http://www.ietf.org/id/all_id2.txt"); //input url of "online rfc index" file first			
//			Properties prop=System.getProperties();//set proxy
//			prop.setProperty("http.proxyHost", "proxy.huawei.com");
//			prop.setProperty("http.proxyPort", "8080");					
			boolean NewRfc=false;//ready to read rfc info
			String RfcNumber=null;
			InputStream in=null;
			if(IDRfcFlag=="rfc"){
				in=urlRFC.openStream();
			}
			if(IDRfcFlag=="id")
				in=urlID.openStream();
			BufferedReader reader=new BufferedReader(new InputStreamReader(in));
			String line=null;
			while ((line=reader.readLine())!=null){
				if(IDRfcFlag=="rfc"){
					if(line.contains("INDEX")==true)
						IndexCounter++;
					if(IndexCounter>=2){
						if(line.length()>=4){
							Pattern p;
							Matcher m;
							p=Pattern.compile("\\D");
							m=p.matcher(line.substring(0, 4));
							if(m.lookingAt()==false && line.substring(0, 4).compareTo(latestLocalRfc)>0){
								NewRfc=true;
								latestLocalRfc=line.substring(0,4);
								RfcNumber="http://www.ietf.org/rfc/rfc"+line.substring(0,4)+".txt";
								writeOneLine(line,RfcIndexLocal);
								continue;
							}
						}
					}
					if(IndexCounter>=2 && NewRfc==true && line.length()!=0)
						writeOneLine(line,RfcIndexLocal);
					if(IndexCounter>=2 && NewRfc==true && line.length()==0){
						readAbstract(RfcNumber,RfcIndexLocal);
						writeOneLine(line,RfcIndexLocal);	
						NewRfc=false;						
					}
				}
				if(IDRfcFlag=="id")
					if(line.startsWith("#")==false && line.contains("Expired")==false && line.contains("Replaced")==false && line.contains("\tRFC\t")==false && line.substring(0,17).compareTo(latestLocalID)>0){
						writeOneLine(line,IDIndexLocal);
						RfcNumber="http://www.ietf.org/id/"+line.substring(0,line.indexOf("\t"))+".txt";
						readAbstract(RfcNumber,IDIndexLocal);
					}
			}	
				
		}catch(MalformedURLException e1){
				urltext.setText(""+e1);
				return;
				}
			catch(IOException e1){
				urltext.setText(""+e1);
				return;
				}
		JOptionPane.showMessageDialog(firstwin, "read finish");						
	}
		public void readAbstract(String rfcNo,File localIndex){
			int n=-1;
			try{
				URL urlRfc=new URL(rfcNo);//online URL
//				Properties prop=System.getProperties();//set proxy
//				prop.setProperty("http.proxyHost", "proxy.huawei.com");
//				prop.setProperty("http.proxyPort", "8080");					
				InputStream inRfc=urlRfc.openStream();
				BufferedReader readerRfc=new BufferedReader(new InputStreamReader(inRfc));
				String lineRfc=null;
				boolean readFlag=false,writeFlag=false;
				while((lineRfc=readerRfc.readLine())!=null){
					if(readFlag==true && lineRfc.length()>=1){
						writeOneLine(lineRfc,localIndex);
						writeFlag=true;//start to write
					}
					if(lineRfc.contains("Abstract")==true && lineRfc.length()<10){
						readFlag=true;
						writeOneLine("Abstract",localIndex);
					}
					if(writeFlag==true && lineRfc.length()<2)
						break;
				}
				
			}catch(MalformedURLException e1){
				urltext.setText(""+e1);
				return;
				}
			catch(IOException e1){
				urltext.setText(""+e1);
				return;
				}

		}
	
	}
}//END OF BOK WINDOW
