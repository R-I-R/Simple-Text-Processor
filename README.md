# Simple-Text-Processor
procesador de texto en desarrollo

package Respaldo;

import javax.swing.*;
import javax.swing.event.*;
import javax.swing.text.StyledEditorKit;

import java.awt.*;
import java.awt.event.*;
import java.awt.image.BufferedImage;

public class Procesador_texto_01 {
	public static void main (String[] arg){
		Marco M = new Marco();
	}
}

class Marco extends JFrame{
	
	public Marco(){
		
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setTitle("Simple Text Processor");
		setBounds(500,200,500,400);
		setResizable(false);
		setIconImage(Toolkit.getDefaultToolkit().getImage(ClassLoader.getSystemResource("imgprotext/logo.gif")));
		
		Panel P = new Panel();
		add(P);
		
		setVisible(true);
	}
}

class Panel extends JPanel{
	
	private boolean select = false;
	private int Modificador = 0, Tamaño = 12, start = 0, end = 0;
	private String Fuente = "Dialog";
	private JTextPane area;
	private JComboBox colores;
	
	public Panel(){
		
	//Layouts
		setLayout(new BorderLayout());
		JPanel MenuL = new JPanel(new GridLayout(2,1));
		JMenuBar MenuL1 = new JMenuBar();
		JPanel MenuL2 = new JPanel(new FlowLayout(FlowLayout.LEFT));
		area = new JTextPane();
		JScrollPane scrol = new JScrollPane(area);
		MenuL.add(MenuL1);
		MenuL.add(MenuL2);

//Selector de Fuentes
		StyledEditorKit style = new StyledEditorKit();
		
		JComboBox fuentes = new JComboBox();
	//	fuentes.setMinimumSize(new Dimension(100,20));
	//	fuentes.setMaximumSize(new Dimension(100,20));
		
		for(String a:GraphicsEnvironment.getLocalGraphicsEnvironment().getAvailableFontFamilyNames()){
			fuentes.addItem(a);
		}
		fuentes.setSelectedItem("Dialog");
		fuentes.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				Fuente = fuentes.getSelectedItem().toString(); 
				area.setFont(new Font(Fuente,Modificador,Tamaño));
			}		
		});
		
		MenuL2.add(fuentes);

//Selector de tamaño

		JSpinner tamaño = new JSpinner(new SpinnerNumberModel(12,1,72,2));
		tamaño.setPreferredSize(new Dimension(40,20));
		
		tamaño.addChangeListener(new ChangeListener(){
			public void stateChanged(ChangeEvent e) {
				Tamaño = (int)tamaño.getValue();
				area.setFont(new Font(Fuente,Modificador,Tamaño));
			}
		});
		MenuL2.add(tamaño);	
		
//Selector de color

		colores = new JComboBox();

		class colorus{
			private int cont = 0;
			private ImageIcon[] vecIm = new ImageIcon[13];
			private Color[] vecCo = new Color[13];
			
			public colorus(){
				setColor(Color.BLACK);
				setColor(Color.BLUE);
				setColor(Color.CYAN);
				setColor(Color.DARK_GRAY);
				setColor(Color.gray);
				setColor(Color.green);
				setColor(Color.LIGHT_GRAY);
				setColor(Color.MAGENTA);
				setColor(Color.ORANGE);
				setColor(Color.PINK);
				setColor(Color.red);
				setColor(Color.white);
				setColor(Color.yellow);
			}
			public void setColor(Color c){
				BufferedImage img = new BufferedImage(10,10,BufferedImage.TYPE_INT_RGB);
				Graphics2D g = img.createGraphics();
				g.setColor(c);
				g.fillRect(0, 0, 10, 10);
				ImageIcon colors = new ImageIcon(img);
				colores.addItem(colors);
				vecIm[cont] = colors;
				vecCo[cont] = c;
				cont++;
			}

			public Color getColor(ImageIcon i){
				boolean Col = true;
				int a;
				for(a = 0; a < 13 && Col; a ++){
					if(i.equals(vecIm[a])) Col = false;
				}
				if(!Col) return vecCo[a-1];
				else{return Color.black;}
			}
		}
		colorus c = new colorus();
		
		colores.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent e) {
				area.setForeground(c.getColor((ImageIcon)colores.getSelectedItem()));
			}
			
		});

		MenuL2.add(colores);

//Modificadores de apariencia del texto

		JCheckBox Negrita = new JCheckBox("Negrita");
		JCheckBox Cursiva = new JCheckBox("Cursiva");
				
		class Box implements ActionListener{
			public void actionPerformed(ActionEvent e) {
				Modificador = 0;
						
				if(Negrita.isSelected()) Modificador += Font.BOLD;
				if(Cursiva.isSelected()) Modificador += Font.ITALIC;
						
				area.setFont(new Font(Fuente, Modificador, Tamaño));		
			}
					
		}
		Negrita.addActionListener(new Box());
		Cursiva.addActionListener(new Box());
				
		MenuL2.add(Negrita);
		MenuL2.add(Cursiva);

//boton ayuda

		JMenu ayuda = new JMenu("Ayuda");
		
		JMenuItem acercade = new JMenuItem("Acerca de",new ImageIcon(ClassLoader.getSystemResource("imgprotext/acercade.gif")));
		acercade.addActionListener(new ActionListener(){
			public void actionPerformed(ActionEvent arg0) {
				class Macerca extends JDialog{
					public Macerca(){
						setDefaultCloseOperation(JFrame.DISPOSE_ON_CLOSE);
						setTitle("Acerca de");
						setSize(250,100);
						setResizable(false);
						setLocationRelativeTo(null);
						setIconImage(Toolkit.getDefaultToolkit().getImage(ClassLoader.getSystemResource("imgprotext/acercade.gif")));
						getContentPane().setLayout(new BorderLayout());
						{
							JLabel lab = new JLabel("Simple Text Processor",new ImageIcon(ClassLoader.getSystemResource("imgprotext/logo.gif")),0);
							JLabel lab2 = new JLabel("  Version (0.1)    Hecho por: SetUser(RIR);");
							getContentPane().add(lab,BorderLayout.CENTER);
							getContentPane().add(lab2,BorderLayout.SOUTH);
						}
						setVisible(true);
					}
				}
				Macerca acerca = new Macerca();
			}
		});
		ayuda.add(acercade);
		MenuL1.add(ayuda);

//Añadir los elementos al panel

		add(scrol, BorderLayout.CENTER);
		add(MenuL,BorderLayout.NORTH);
	}//cierro constructor

}//termino programa
