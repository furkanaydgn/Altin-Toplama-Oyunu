/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package altin_oyunu;


import java.awt.Graphics;
import java.awt.Color;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.FocusEvent;
import java.awt.event.FocusListener;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.awt.event.MouseMotionListener;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import static java.lang.Math.abs;
import java.util.ArrayList;
import java.util.Random;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.swing.JFrame;
import javax.swing.BorderFactory;
import javax.swing.JButton;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.JSlider;
import javax.swing.border.Border;
import javax.swing.border.EtchedBorder;
import javax.swing.event.ChangeEvent;
import javax.swing.event.ChangeListener;
import javax.swing.JComboBox;
import javax.swing.JScrollPane;
import javax.swing.JTable;
import javax.swing.JTextField;

public class Altin_Oyunu {

    public static File dosya;
    public static BufferedWriter writer;
    public static FileWriter dosya_yaz;
    public static ArrayList<Integer> A_ArrayList, B_ArrayList, C_ArrayList, D_ArrayList;
    JFrame frame;
    private int satirX = 20, sutunY = 20, adima = 1, adimb = 1, adimc = 1, adimd = 1, harcanan_altin_A = 0, toplanan_altin_A = 0, harcanan_altin_B = 0, toplanan_altin_B = 0, harcanan_altin_C = 0, toplanan_altin_C = 0, harcanan_altin_D = 0, toplanan_altin_D = 0;
    public int a_altin_kontrol = 0, b_altin_kontrol = 0;
    public int birinci = 0, ikinci = 0, ucuncu = 0;
    public int A_hamle_maliyet = 5, A_hedef_maliyet = 5, B_hedef_maliyet = 10, B_hamle_maliyet = 5, C_hamle_maliyet = 5, C_hedef_maliyet = 15, D_hamle_maliyet = 5, D_hedef_maliyet = 20;
    public int adim_sayisi = 3, altin_sayisi = 0;
    public int degs, uzunluk_A, uzunluk_B, uzunluk_C;
    public int A_altin_uzaklik = 0;
    public next[] ileri, ileri1, ileri2, yol, altin_koordinat, altin_koordinatC, yol_C, yol_D, ileri3, altin_koordinatD;
    public ArrayList<next>[] al, al_C, al_D;
    private int gecikme = 30;
    private double G_altin = .1;
    private double Altin_yuzdesi = .2;
    private double Altin_adedi = (satirX * sutunY) * .2;
    private int A_baslangicX = -1;
    private int A_baslangicY = -1;
    private int B_baslangicX = -1;
    private int B_baslangicY = -1;
    private int araclar = 0;
    private int length = 0, length_D = 0;
    private int WIDTH = 1050;
    private final int HEIGHT = 800;
    private final int MSIZE = 690;
    private int CSIZEX = MSIZE / satirX;
    private int CSIZEY = MSIZE / sutunY;
    private int a_X, a_Y, b_X, b_Y, c_x, c_Y, d_X, d_Y, oyuncu_sira = 1, a_en_son_kalinan_adim = 0, b_altina_uzaklik = 0, c_altina_uzaklik = 0, d_altina_uzaklik = 0, en_son_kal_adim_B = 0, en_son_kal_adim_D = 0,
            abeyaztut = 0, bbeyaztut = 0, cbeyaztut = 0, dbeyaztut = 0, adm2 = 1, adm1 = 1, adm3 = 1, adm = 1, renkY_tut, renkK_tut, renkD_tut, kontrol = 0, Genel_Puan = 200, cx, cy, dx, dy, en_son_kal_adim_C = 0, renkC_tut = 0, adim_C = 1, adim_D = 1, adim_D1, adim_C1 = 1, degs_C = 0, degs_D = 0;
    private int bx, by, A_oyuncu = 200, B_oyuncu = 200, C_oyuncu = 200, D_oyuncu = 200, a_kontrol, b_kontrol, c_kontrol, d_kontrol, d_bitis_kontrol = 0;
    private boolean a, b, c, d, a_oyun = true, b_oyun = true, c_oyun = true, d_oyun = true;
    private int[][] maliyet;
    public next[][] Next;
    public int[] path, kar_hesabi, Manh_uzakligi, uzunluk;
    private String[] tools = {"A oyuncusu", "B oyuncusu", "Altin", "C oyuncusu", "D oyuncusu"};
    private boolean devamMi = false;
    Node[][] map, B_Sanal_Map, C_Sanal_Map, D_Sanal_Map, D_Map;
    Algoritma Alg = new Algoritma();
    Random r = new Random();
    JSlider Altin_Oran = new JSlider(1, 100, 20);
    JSlider G_Altin_Oran = new JSlider(1, 100, 10);
    JTextField Adim_S = new JTextField("" + adim_sayisi);
    JLabel Adim_Label = new JLabel("Adim Sayisi : ");
    JLabel T_Puan = new JLabel("Toplam Puanı : ");
    JTextField T_Puan1 = new JTextField("" + Genel_Puan);
    JLabel toolL = new JLabel("Oyuncular");

    JLabel Altin_Label = new JLabel("Altin sayısı :");
    JLabel G_Altin_Text = new JLabel("G.Altin sayi:");
    JLabel G_Altin_yognluk = new JLabel(G_Altin_Oran.getValue() + "%");
    JLabel Altin_yognluk = new JLabel(Altin_Oran.getValue() + "%");

    JLabel A = new JLabel("A oyuncusu : " + A_oyuncu);
    JLabel B = new JLabel("B oyuncusu : " + B_oyuncu);
    JLabel C = new JLabel("C oyuncusu : " + C_oyuncu);
    JLabel D = new JLabel("D oyuncusu : " + D_oyuncu);
    JLabel A_hamle_label = new JLabel("A hamle maliyet : ");
    JLabel A_hedef_label = new JLabel("A hedef maliyet : ");
    JLabel B_hamle_label = new JLabel("B hamle maliyet : ");
    JLabel B_hedef_label = new JLabel("B hedef maliyet : ");
    JLabel C_hamle_label = new JLabel("C hamle maliyet : ");
    JLabel C_hedef_label = new JLabel("C hedef maliyet : ");
    JLabel D_hamle_label = new JLabel("D hamle maliyet : ");
    JLabel D_hedef_label = new JLabel("D hedef maliyet : ");
    JTextField A_hamle_field = new JTextField("" + A_hamle_maliyet);
    JTextField A_hedef_field = new JTextField("" + A_hedef_maliyet);
    JTextField B_hamle_field = new JTextField("" + B_hamle_maliyet);
    JTextField B_hedef_field = new JTextField("" + B_hedef_maliyet);
    JTextField C_hamle_field = new JTextField("" + C_hamle_maliyet);
    JTextField C_hedef_field = new JTextField("" + C_hedef_maliyet);
    JTextField D_hamle_field = new JTextField("" + D_hamle_maliyet);
    JTextField D_hedef_field = new JTextField("" + D_hedef_maliyet);
    JButton searchB = new JButton("Oyuna Başla");
    JButton genMapB = new JButton("Altınları ata");
    JButton Ayarlar = new JButton("Ayarlar");
    JComboBox toolBx = new JComboBox(tools);
    JPanel menu = new JPanel();
    JPanel menu2 = new JPanel();
    JPanel ayarlar = new JPanel();
    JLabel boyutA = new JLabel("Satır sayısı : ");
    JLabel boyutB = new JLabel("Sütun sayısı : ");
    JTextField satir = new JTextField("" + satirX);
    JTextField sutun = new JTextField("" + sutunY);
    JTable tablo = new JTable();
    Font fon1 = new Font("SansSerif", Font.BOLD, 15);
    Map canvas;
    Border loweredetched = BorderFactory.createEtchedBorder(EtchedBorder.LOWERED);

    public void ozet_Tablo() {
        JFrame f;

        f = new JFrame();
        String data[][] = {{"A", "" + adima, "" + harcanan_altin_A, "" + A_oyuncu, "" + toplanan_altin_A},
        {"B", "" + adimb, "" + harcanan_altin_B, "" + B_oyuncu, "" + toplanan_altin_B},
        {"C", "" + adimc, "" + harcanan_altin_C, "" + C_oyuncu, "" + toplanan_altin_C},
        {"D", "" + adimd, "" + harcanan_altin_D, "" + D_oyuncu, "" + toplanan_altin_D}};
        String column[] = {"Oyuncu_adi", "Toplam Adim", "Harcanan Altin", "Kasa Altin", "Toplanan altin"};
        JTable jt = new JTable(data, column);
        jt.setBounds(30, 40, 400, 700);
        JScrollPane sp = new JScrollPane(jt);
        f.add(sp);
        f.setResizable(false);
        f.setTitle("Oyun Ozet Tablo");
        f.setSize(700, 170);
        f.setVisible(true);

    }

    public void maliyet() {
        int i, j = 0;
        kar_hesabi = new int[degs];
        for (i = 0; i < degs; i++) {
            kar_hesabi[i] = (maliyet[altin_koordinat[i].x][altin_koordinat[i].y]) - ((int) Math.ceil((path[i] / (double) adim_sayisi)) * B_hamle_maliyet);   
        }
        int minimum = kar_hesabi[0];
        for (i = 0; i < degs; i++) {
            if (kar_hesabi[i] > minimum) {
                minimum = kar_hesabi[i];
                j = i;
            }
        }
        yol = new next[al[j].size()];
        b_altina_uzaklik = al[j].size();
        for (int k = 0; k < al[j].size(); k++) {
            yol[k] = new next(al[j].get(k).x, al[j].get(k).y);
        }
        map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].setType(2);
        canvas.repaint();
        delay1(500);
        map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].setType(5);
        canvas.repaint();
        B_oyuncu -= B_hedef_maliyet;
        harcanan_altin_B += B_hedef_maliyet;

    }

    public void Maliyet_C() {
        int i, j = 0;
        kar_hesabi = new int[degs_C];

        for (i = 0; i < degs_C; i++) {
            kar_hesabi[i] = (maliyet[altin_koordinatC[i].x][altin_koordinatC[i].y]) - ((int) Math.ceil((path[i] / (double) adim_sayisi)) * C_hamle_maliyet);     

        }

        int minimum = kar_hesabi[0];
        for (i = 0; i < degs_C; i++) {
            if (kar_hesabi[i] > minimum) {
                minimum = kar_hesabi[i];
                j = i;
            }

        }
        yol_C = new next[al_C[j].size()];
        c_altina_uzaklik = al_C[j].size();
        for (int k = 0; k < al_C[j].size(); k++) {
            yol_C[k] = new next(al_C[j].get(k).x, al_C[j].get(k).y);
          
        }
        map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].setType(2);
        canvas.repaint();
        delay1(500);
        map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].setType(5);
        canvas.repaint();
        C_oyuncu -= C_hedef_maliyet;
        harcanan_altin_C += C_hedef_maliyet;
    }

    public void Maliyet_D() {
        int i, j = 0;
        kar_hesabi = new int[degs_D];
       
        for (i = 0; i < degs_D; i++) {
            kar_hesabi[i] = (maliyet[altin_koordinatD[i].x][altin_koordinatD[i].y]) - ((int) Math.ceil((path[i] / (double) adim_sayisi)) * D_hamle_maliyet);
            
        }
        int minimum = kar_hesabi[0];
        for (i = 0; i < degs_D; i++) {
            if (kar_hesabi[i] > minimum) {
                minimum = kar_hesabi[i];
                j = i;
            }

        }
        yol_D = new next[al_D[j].size()];
        d_altina_uzaklik = al_D[j].size();
        for (int k = 0; k < al_D[j].size(); k++) {
            yol_D[k] = new next(al_D[j].get(k).x, al_D[j].get(k).y);
        }
        ileri3 = new next[d_altina_uzaklik];
        for (int m = 0; m < d_altina_uzaklik; m++) {
            ileri3[m] = new next(yol_D[m].x, yol_D[m].y);

        }
        map[ileri3[d_altina_uzaklik - 1].x][ileri3[d_altina_uzaklik - 1].y].setType(2);
        canvas.repaint();
        delay1(500);
        map[ileri3[d_altina_uzaklik - 1].x][ileri3[d_altina_uzaklik - 1].y].setType(5);
        canvas.repaint();
        D_oyuncu -= D_hedef_maliyet;
        harcanan_altin_D += D_hedef_maliyet;
    }

    public static void main(String[] args) {
        dosya = new File("Oyun-Ozet.txt");
        try {
            dosya_yaz = new FileWriter(dosya);
            writer = new BufferedWriter(dosya_yaz);

        } catch (IOException e) {
            System.out.println("Dosya yazma işlemi başarısız  " + e);
        }

        A_ArrayList = new ArrayList<>();
        B_ArrayList = new ArrayList<>();
        C_ArrayList = new ArrayList<>();
        D_ArrayList = new ArrayList<>();

        new Altin_Oyunu();
    }

    public void degersifirla() {
        if (b == true) {
            for (int km = 0; km < satirX; km++) {
                for (int kn = 0; kn < sutunY; kn++) {
                    if (map[km][kn].getType() == 4 || B_Sanal_Map[km][kn].getType() == 4 || map[km][kn].getType() == 5) {
                        B_Sanal_Map[km][kn].setHops(-1);
                    }
                }
            }
        }
        if (c == true) {
            for (int km = 0; km < satirX; km++) {
                for (int kn = 0; kn < sutunY; kn++) {
                    if (map[km][kn].getType() == 4 || C_Sanal_Map[km][kn].getType() == 4 || map[km][kn].getType() == 5) {
                        C_Sanal_Map[km][kn].setHops(-1);
                    }
                }
            }
        }
        if (d == true) {
            for (int km = 0; km < satirX; km++) {
                for (int kn = 0; kn < sutunY; kn++) {
                    if (map[km][kn].getType() == 4 || D_Sanal_Map[km][kn].getType() == 4 || map[km][kn].getType() == 5) {  
                        D_Sanal_Map[km][kn].setHops(-1);
                    }
                }
            }
        }
    }

    public Altin_Oyunu() {
        a = true;
        harita_temizle();
        initialize();
    }

    public void altin_degerAta() {
        maliyet = new int[satirX][sutunY];
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                if (map[i][j].getType() == 5 || map[i][j].getType() == 6) {
                    maliyet[i][j] = (1 + r.nextInt(4)) * 5;
                }
            }
        }
    }

    public void C_hesapla() {

        C_Sanal_Map = new Node[satirX][sutunY];
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                C_Sanal_Map[i][j] = new Node(map[i][j].getType(), map[i][j].getX(), map[i][j].getY());
                C_Sanal_Map[i][j].setHops(map[i][j].getHops());
            }
        }
        int G_sayisi = 0, A_sayisi = 0;
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                if (C_Sanal_Map[i][j].getType() == 6) {
                    G_sayisi++;
                }
                if (C_Sanal_Map[i][j].getType() == 5) {
                    A_sayisi++;
                }
            }
        }
        Manh_uzakligi = new int[G_sayisi];
        int sayac = 0;
        int[] manh_koordintX = new int[G_sayisi];
        int[] manh_koordintY = new int[G_sayisi];
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                if (C_Sanal_Map[i][j].getType() == 6) {
                    Manh_uzakligi[sayac] = abs(C_Sanal_Map[i][j].x - C_Sanal_Map[cx][cy].x) + abs(C_Sanal_Map[i][j].y - C_Sanal_Map[cx][cy].y);
                    manh_koordintX[sayac] = i;
                    manh_koordintY[sayac] = j;
                    sayac++;
                }
            }
        }

        int temp;
        for (int i = 0; i < G_sayisi - 1; i++) {
            for (int j = i + 1; j < G_sayisi; j++) {
                if (Manh_uzakligi[i] > Manh_uzakligi[j]) {
                    temp = Manh_uzakligi[j];
                    Manh_uzakligi[j] = Manh_uzakligi[i];
                    Manh_uzakligi[i] = temp;

                    temp = manh_koordintX[j];
                    manh_koordintX[j] = manh_koordintX[i];
                    manh_koordintX[i] = temp;

                    temp = manh_koordintY[j];
                    manh_koordintY[j] = manh_koordintY[i];
                    manh_koordintY[i] = temp;
                }
            }
        }

        int ara_ver = 0;
        for (int i = 0; i < G_sayisi; i++) {
            if (ara_ver == 2) {

                break;
            }
            ara_ver++;
            map[manh_koordintX[i]][manh_koordintY[i]].setType(5);
            map[manh_koordintX[i]][manh_koordintY[i]].setHops(-1);
            C_Sanal_Map[manh_koordintX[i]][manh_koordintY[i]].setType(5);
            C_Sanal_Map[manh_koordintX[i]][manh_koordintY[i]].setHops(-1);
        }
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                C_Sanal_Map[i][j] = new Node(map[i][j].getType(), map[i][j].getX(), map[i][j].getY());
                C_Sanal_Map[i][j].setHops(map[i][j].getHops());
            }
        }
        G_sayisi = 0;
        A_sayisi = 0;
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                if (C_Sanal_Map[i][j].getType() == 6) {
                    G_sayisi++;
                }
                if (C_Sanal_Map[i][j].getType() == 5) {
                    A_sayisi++;
                }
            }
        }
        degs_C = A_sayisi;
        altin_koordinatC = new next[A_sayisi];
        path = new int[A_sayisi];
        devamMi = true;
        Alg.Dijkstra();

        al_C = new ArrayList[A_sayisi];

        for (int i = 0; i <= A_sayisi; i++) {

            devamMi = true;
            Next = new next[degs_C][length];
            al_C[i] = new ArrayList<next>();

            degersifirla();
            for (int j = 0; j < length; j++) {

                Next[i][j] = new next(ileri2[j].x, ileri2[j].y);
                al_C[i].add(Next[i][j]);
            }
            altin_koordinatC[i] = new next(al_C[i].get(length - 1).x, al_C[i].get(length - 1).y);
            path[i] = length;
            if (A_sayisi == i + 1) {
                break;
            }
            Alg.Dijkstra();
        }
        C_ArrayList.add(C_Sanal_Map[cx][cy].x);
        C_ArrayList.add(C_Sanal_Map[cx][cy].y);
        Maliyet_C();
        devamMi = true;
    }

    public void hesapla_D() {
        d_bitis_kontrol = 0;
        D_Sanal_Map = new Node[satirX][sutunY];

        if (birinci == 1) {
           
            if (map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].getType() != 0 && map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].getType() != 1 && map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].getType() != 9) {

                d_bitis_kontrol = 1;

                for (int mn = 0; mn < satirX; mn++) {
                    for (int j = 0; j < sutunY; j++) {
                        D_Sanal_Map[mn][j] = new Node(3, map[mn][j].getX(), map[mn][j].getY());

                        D_Sanal_Map[mn][j].setHops(-1);
                    }
                }

                D_Sanal_Map[dx][dy].setType(10);
                D_Sanal_Map[dx][dy].setHops(0);
                D_Sanal_Map[A_baslangicX][A_baslangicY].setType(0);
                D_Sanal_Map[A_baslangicX][A_baslangicY].setHops(0);
                D_Sanal_Map[bx][by].setType(1);
                D_Sanal_Map[bx][by].setHops(0);
                D_Sanal_Map[cx][cy].setType(9);
                D_Sanal_Map[cx][cy].setHops(0);
                D_Sanal_Map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].setType(5);
                devamMi = true;
                Alg.Dijkstra();
                D_oyuncu -= D_hedef_maliyet;
                harcanan_altin_D += D_hedef_maliyet;
            }
            birinci = 0;
            ikinci = 0;
            ucuncu = 0;
        } else if (ikinci == 1) {
           
            if (map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].getType() != 0 && map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].getType() != 1 && map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].getType() != 9) {

                d_bitis_kontrol = 1;

                for (int mn = 0; mn < satirX; mn++) {
                    for (int j = 0; j < sutunY; j++) {
                        D_Sanal_Map[mn][j] = new Node(3, map[mn][j].getX(), map[mn][j].getY());

                        D_Sanal_Map[mn][j].setHops(-1);
                    }
                }

                D_Sanal_Map[dx][dy].setType(10);
                D_Sanal_Map[dx][dy].setHops(0);
                D_Sanal_Map[A_baslangicX][A_baslangicY].setType(0);
                D_Sanal_Map[A_baslangicX][A_baslangicY].setHops(0);
                D_Sanal_Map[bx][by].setType(1);
                D_Sanal_Map[bx][by].setHops(0);
                D_Sanal_Map[cx][cy].setType(9);
                D_Sanal_Map[cx][cy].setHops(0);
                D_Sanal_Map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].setType(5);
                devamMi = true;
                Alg.Dijkstra();
                D_oyuncu -= D_hedef_maliyet;
                harcanan_altin_D += D_hedef_maliyet;
            }
            birinci = 0;
            ikinci = 0;
            ucuncu = 0;
        } else if (ucuncu == 1) {
           
            if (map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].getType() != 0 && map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].getType() != 1 && map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].getType() != 9) {

                d_bitis_kontrol = 1;

                for (int mn = 0; mn < satirX; mn++) {
                    for (int j = 0; j < sutunY; j++) {
                        D_Sanal_Map[mn][j] = new Node(3, map[mn][j].getX(), map[mn][j].getY());

                        D_Sanal_Map[mn][j].setHops(-1);
                    }
                }

                D_Sanal_Map[dx][dy].setType(10);
                D_Sanal_Map[dx][dy].setHops(0);
                D_Sanal_Map[A_baslangicX][A_baslangicY].setType(0);
                D_Sanal_Map[A_baslangicX][A_baslangicY].setHops(0);
                D_Sanal_Map[bx][by].setType(1);
                D_Sanal_Map[bx][by].setHops(0);
                D_Sanal_Map[cx][cy].setType(9);
                D_Sanal_Map[cx][cy].setHops(0);
                D_Sanal_Map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].setType(5);
                devamMi = true;
                Alg.Dijkstra();
                D_oyuncu -= D_hedef_maliyet;
                harcanan_altin_D += D_hedef_maliyet;
            }
            birinci = 0;
            ikinci = 0;
            ucuncu = 0;
        } else {

            d_bitis_kontrol = 2;

            for (int mn = 0; mn < satirX; mn++) {
                for (int j = 0; j < sutunY; j++) {
                    D_Sanal_Map[mn][j] = new Node(3, map[mn][j].getX(), map[mn][j].getY());
                    D_Sanal_Map[mn][j].setHops(-1);

                    if ((mn == ileri[A_altin_uzaklik - 1].x && ileri[A_altin_uzaklik - 1].y == j) || (mn == yol[b_altina_uzaklik - 1].x && yol[b_altina_uzaklik - 1].y == j) || (mn == yol_C[c_altina_uzaklik - 1].x && yol_C[c_altina_uzaklik - 1].y == j)) {

                    } else {
                        D_Sanal_Map[mn][j] = new Node(map[mn][j].getType(), map[mn][j].getX(), map[mn][j].getY());
                        D_Sanal_Map[mn][j].setHops(map[mn][j].getHops());
                    
                    }

                }
                
            }
         
            int A_sayisi = 0;
            for (int i = 0; i < satirX; i++) {
                for (int j = 0; j < sutunY; j++) {
                    if (D_Sanal_Map[i][j].getType() == 5) {
                        A_sayisi++;
                    }
                }
            }
          
            degs_D = A_sayisi;
            altin_koordinatD = new next[A_sayisi];
            path = new int[A_sayisi];
            devamMi = true;
         
            Alg.Dijkstra();
            degersifirla();
            al_D = new ArrayList[A_sayisi];

            for (int i = 0; i <= A_sayisi; i++) {

                devamMi = true;
                Next = new next[degs_D][length_D];
                al_D[i] = new ArrayList<next>();
              
                degersifirla();
                
         
                for (int j = 0; j < length_D; j++) {

                    Next[i][j] = new next(ileri3[j].x, ileri3[j].y);
                    al_D[i].add(Next[i][j]);
                }
                altin_koordinatD[i] = new next(al_D[i].get(length_D - 1).x, al_D[i].get(length_D - 1).y);
               
                path[i] = length_D;
                if (A_sayisi == i + 1) {
                    break;
                }
                Alg.Dijkstra();
            }
           
                
            
            
            D_ArrayList.add(D_Sanal_Map[dx][dy].x);
            D_ArrayList.add(D_Sanal_Map[dx][dy].y);
            Maliyet_D();
            devamMi = true;

        }

    }

    public void hesapla() {
        B_Sanal_Map = new Node[satirX][sutunY];
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                B_Sanal_Map[i][j] = new Node(map[i][j].getType(), map[i][j].getX(), map[i][j].getY());
                B_Sanal_Map[i][j].setHops(map[i][j].getHops());
            }
        }
        int sayac = 0;
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                if (B_Sanal_Map[i][j].getType() == 5) {
                    sayac++;
                }
            }
        }
        degs = sayac;
        altin_koordinat = new next[sayac];
        path = new int[sayac];
        devamMi = true;
        Alg.Dijkstra();

        al = new ArrayList[sayac];

        for (int i = 0; i <= sayac; i++) {

            devamMi = true;
            Next = new next[degs][length];
            al[i] = new ArrayList<next>();

            degersifirla();
            for (int j = 0; j < length; j++) {

                Next[i][j] = new next(ileri1[j].x, ileri1[j].y);
                al[i].add(Next[i][j]);
            }
            altin_koordinat[i] = new next(al[i].get(length - 1).x, al[i].get(length - 1).y);
            path[i] = length;
            if (sayac == i + 1) {
                break;
            }
            Alg.Dijkstra();
        }
         B_ArrayList.add(B_Sanal_Map[bx][by].x);
         B_ArrayList.add(B_Sanal_Map[bx][by].y); 
        maliyet();
        devamMi = true;

    }

    public void altin_ata() {
        harita_temizle();
        for (int i = 0; i < Altin_adedi; i++) {
            Node current;
            do {
                int x = r.nextInt(satirX);
                int y = r.nextInt(sutunY);
                current = map[x][y];

            } while (current.getType() == 5);
            current.setType(5);
        }

    }

    public void GizliAltin() {

        for (int i = 0; i < Altin_adedi * G_altin; i++) {
            Node current;
            do {
                int x = r.nextInt(satirX);

                int y = r.nextInt(sutunY);

                current = map[x][y];

            } while (current.getType() == 3 || current.getType() == 6);
            if (current.getType() == 5) {
                current.setType(6);
            }
        }

    }

    public void harita_temizle() {

        map = new Node[satirX][sutunY];
        for (int x = 0; x < satirX; x++) {
            for (int y = 0; y < sutunY; y++) {
                map[x][y] = new Node(3, x, y);

            }
        }
        
    }

    private void initialize() {
        frame = new JFrame();
        frame.setVisible(true);
        frame.setResizable(false);
        frame.setSize(WIDTH, HEIGHT);
        frame.setTitle("Altin bulma oyunu");
        frame.setLocationRelativeTo(null);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.getContentPane().setLayout(null);

        frame.getContentPane().setBackground(Color.darkGray);

        menu2.setBorder(BorderFactory.createTitledBorder(loweredetched, "Controls"));
        menu.setBorder(BorderFactory.createTitledBorder(loweredetched, "Controls"));

        int space = 25;
        int buff = 45;

        menu.setLayout(null);
        menu.setBounds(10, 10, 210, 750);

        searchB.setBounds(40, space, 120, 25);
        menu.add(searchB);
        space += buff;

        genMapB.setBounds(40, space, 120, 25);
        menu.add(genMapB);
        space += buff;

        Ayarlar.setBounds(40, space, 120, 25);
        menu.add(Ayarlar);
        space += 40;

        toolL.setBounds(40, space, 120, 25);
        menu.add(toolL);
        space += 25;

        toolBx.setBounds(40, space, 120, 25);
        menu.add(toolBx);
        space += buff;

        A.setBounds(15, space, 200, 25);
        menu.add(A);
        space += buff;
        B.setBounds(15, space, 200, 25);
        menu.add(B);
        space += buff;
        C.setBounds(15, space, 200, 25);
        menu.add(C);
        space += buff;
        D.setBounds(15, space, 200, 25);
        menu.add(D);
        menu.setBackground(Color.lightGray);
        frame.getContentPane().add(menu);
        canvas = new Map();
        canvas.setBounds(270, 35, MSIZE + 1, MSIZE + 1);
        frame.getContentPane().add(canvas);

        searchB.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
              
                if ((A_baslangicX > -1 && A_baslangicY > -1) && (B_baslangicX > -1 && B_baslangicY > -1)) {

                    devamMi = true;

                }

            }
        });
        genMapB.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                altin_ata();
                GizliAltin();
                altin_degerAta();
                Guncelle();
            }
        });
        Adim_S.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {

                adim_sayisi = Integer.parseInt(Adim_S.getText());
                Guncelle();
            }

        });
        T_Puan1.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                Guncelle1();
                Guncelle();
            }

        });
        Ayarlar.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                JFrame jframe = new JFrame();
                jframe.setSize(580, 600);
                jframe.setLocationRelativeTo(null);
                jframe.setResizable(false);
                jframe.setVisible(true);
                jframe.getContentPane().setLayout(null);
                menu2.setBorder(BorderFactory.createTitledBorder(loweredetched, "Ayarlar"));
                menu2.setLayout(null);
                menu2.setBounds(20, 10, 250, 550);

                boyutA.setBounds(20, 40, 100, 20);
                boyutA.setFont(fon1);
                menu2.add(boyutA);

                satir.setBounds(130, 40, 50, 20);
                menu2.add(satir);

                boyutB.setBounds(20, 65, 100, 20);
                boyutB.setFont(fon1);
                menu2.add(boyutB);

                sutun.setBounds(130, 65, 50, 20);
                menu2.add(sutun);

                Altin_Label.setBounds(20, 170, 100, 25);
                Altin_Label.setFont(fon1);
                menu2.add(Altin_Label);

                Altin_Oran.setMajorTickSpacing(5);
                Altin_Oran.setBounds(100, 170, 100, 25);
                menu2.add(Altin_Oran);

                Altin_yognluk.setBounds(200, 170, 100, 25);
                menu2.add(Altin_yognluk);

                G_Altin_Text.setBounds(20, 200, 125, 25);
                G_Altin_Text.setFont(fon1);
                menu2.add(G_Altin_Text);

                G_Altin_Oran.setMajorTickSpacing(5);
                G_Altin_Oran.setBounds(100, 200, 100, 25);
                menu2.add(G_Altin_Oran);

                G_Altin_yognluk.setBounds(200, 200, 100, 25);
                menu2.add(G_Altin_yognluk);
                T_Puan.setBounds(20, 280, 120, 25);
                T_Puan.setFont(fon1);
                menu2.add(T_Puan);

                T_Puan1.setBounds(130, 280, 50, 25);
                menu2.add(T_Puan1);

                Adim_Label.setBounds(20, 310, 120, 25);
                Adim_Label.setFont(fon1);
                menu2.add(Adim_Label);

                Adim_S.setBounds(130, 310, 50, 25);
                menu2.add(Adim_S);

                ayarlar.setBorder(BorderFactory.createTitledBorder(loweredetched, "Oyun içi ayarlar"));
                ayarlar.setLayout(null);
                ayarlar.setBounds(300, 10, 250, 550);

                A_hamle_label.setBounds(20, 40, 120, 20);
                A_hamle_label.setFont(fon1);
                ayarlar.add(A_hamle_label);

                A_hamle_field.setBounds(150, 40, 50, 20);
                ayarlar.add(A_hamle_field);

                A_hedef_label.setBounds(20, 70, 120, 20);
                A_hedef_label.setFont(fon1);
                ayarlar.add(A_hedef_label);

                A_hedef_field.setBounds(150, 70, 50, 20);
                ayarlar.add(A_hedef_field);

                B_hamle_label.setBounds(20, 120, 125, 20);
                B_hamle_label.setFont(fon1);
                ayarlar.add(B_hamle_label);

                B_hamle_field.setBounds(150, 120, 50, 20);
                ayarlar.add(B_hamle_field);

                B_hedef_label.setBounds(20, 150, 125, 20);
                B_hedef_label.setFont(fon1);
                ayarlar.add(B_hedef_label);

                B_hedef_field.setBounds(150, 150, 50, 20);
                ayarlar.add(B_hedef_field);

                C_hamle_label.setBounds(20, 200, 125, 20);
                C_hamle_label.setFont(fon1);
                ayarlar.add(C_hamle_label);

                C_hamle_field.setBounds(150, 200, 50, 20);
                ayarlar.add(C_hamle_field);

                C_hedef_label.setBounds(20, 230, 125, 20);
                C_hedef_label.setFont(fon1);
                ayarlar.add(C_hedef_label);

                C_hedef_field.setBounds(150, 230, 50, 20);
                ayarlar.add(C_hedef_field);

                D_hamle_label.setBounds(20, 280, 125, 20);
                D_hamle_label.setFont(fon1);
                ayarlar.add(D_hamle_label);

                D_hamle_field.setBounds(150, 280, 50, 20);
                ayarlar.add(D_hamle_field);

                D_hedef_label.setBounds(20, 310, 125, 20);
                D_hedef_label.setFont(fon1);
                ayarlar.add(D_hedef_label);

                D_hedef_field.setBounds(150, 310, 50, 20);
                ayarlar.add(D_hedef_field);

                jframe.getContentPane().add(ayarlar);
                jframe.getContentPane().add(menu2);
                Guncelle();
            }
        });
        satir.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                satirX = Integer.parseInt(satir.getText());
                harita_temizle();
                Guncelle();
            }

        });
        sutun.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                sutunY = Integer.parseInt(sutun.getText());
                harita_temizle();
                Guncelle();
            }

        });
        A_hamle_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                A_hamle_maliyet = Integer.parseInt(A_hamle_field.getText());

            }

        });
        A_hedef_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                A_hedef_maliyet = Integer.parseInt(A_hedef_field.getText());

            }

        });
        B_hamle_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                B_hamle_maliyet = Integer.parseInt(B_hamle_field.getText());

            }

        });
        B_hedef_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                B_hedef_maliyet = Integer.parseInt(B_hedef_field.getText());

            }

        });
        C_hamle_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                C_hamle_maliyet = Integer.parseInt(C_hamle_field.getText());

            }

        });
        C_hedef_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                C_hedef_maliyet = Integer.parseInt(C_hedef_field.getText());

            }

        });
        D_hamle_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                D_hamle_maliyet = Integer.parseInt(D_hamle_field.getText());

            }

        });
        D_hedef_field.addFocusListener(new FocusListener() {
            @Override
            public void focusGained(FocusEvent fe) {

            }

            @Override
            public void focusLost(FocusEvent fe) {
                D_hedef_maliyet = Integer.parseInt(D_hedef_field.getText());

            }

        });
        toolBx.addItemListener(new ItemListener() {
            @Override
            public void itemStateChanged(ItemEvent e) {
                araclar = toolBx.getSelectedIndex();
            }
        });

        Altin_Oran.addChangeListener(new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                Altin_yuzdesi = (double) Altin_Oran.getValue() / 100;
                Guncelle();
            }
        });
        G_Altin_Oran.addChangeListener(new ChangeListener() {
            @Override
            public void stateChanged(ChangeEvent e) {
                G_altin = (double) G_Altin_Oran.getValue() / 100;
                
                Guncelle();
            }
        });
        Oyuna_basla();
    }

    public void Oyuna_basla() {
        while (true) {
            if (devamMi == false) {
                break;
            } else {
                Alg.Dijkstra();
                deger();
            }
        }
        durdur();
    }

    public void durdur() {

        int i = 0;
        while (!devamMi) {
            i++;
            if (i > 500) {
                i = 0;
            }
            try {
                Thread.sleep(1);
            } catch (Exception e) {
            }
        }
        Oyuna_basla();

    }

    public void Guncelle1() {
        Genel_Puan = Integer.parseInt(T_Puan1.getText());
        A_oyuncu = Genel_Puan;
        B_oyuncu = Genel_Puan;
        C_oyuncu = Genel_Puan;
        D_oyuncu = Genel_Puan;
        canvas.repaint();
    }

    public void Guncelle() {

        Altin_adedi = (satirX * sutunY) * Altin_yuzdesi;
        CSIZEX = MSIZE / satirX;
        CSIZEY = MSIZE / sutunY;
        canvas.repaint();
        Adim_S.setText("" + adim_sayisi);
        Altin_yognluk.setText(Altin_Oran.getValue() + "%");
        G_Altin_yognluk.setText(G_Altin_Oran.getValue() + "%");
        A.setText("A oyuncusu : " + A_oyuncu);
        B.setText("B oyuncusu : " + B_oyuncu);
        C.setText("C oyuncusu : " + C_oyuncu);
        D.setText("D oyuncusu : " + D_oyuncu);
    }

 

    public void delay() {
        try {
            Thread.sleep(gecikme);
        } catch (Exception e) {
        }
    }

    public void delay1(int sayi) {
        try {
            Thread.sleep(sayi);
        } catch (Exception e) {
        }
    }

    class Map extends JPanel implements MouseListener, MouseMotionListener {

        public Map() {

            addMouseListener(this);
            addMouseMotionListener(this);
        }

        public void paintComponent(Graphics g) {
            super.paintComponent(g);

            for (int x = 0; x < satirX; x++) {
                for (int y = 0; y < sutunY; y++) {
                    switch (map[x][y].getType()) {
                        case 0:
                            g.setColor(Color.GREEN);
                            break;
                        case 1:
                            g.setColor(Color.RED);
                            break;
                        case 2:

                            g.setColor(Color.BLACK);
                            break;
                        case 3:
                            g.setColor(Color.WHITE);
                            break;
                        case 4:

                            g.setColor(Color.CYAN);
                            break;
                        case 5:

                            g.setColor(Color.YELLOW);

                            break;
                        case 6:

                            g.setColor(Color.ORANGE);
                            break;
                        case 7:

                            g.setColor(Color.BLUE);
                            break;
                        case 8:

                            g.setColor(Color.PINK);
                            break;
                        case 9:

                            g.setColor(Color.darkGray);
                            break;
                        case 10:
                            g.setColor(Color.MAGENTA);
                    }
                    int red = 51;

                    g.fillRect(x * CSIZEX, y * CSIZEY, CSIZEX, CSIZEY);
                    g.setColor(Color.BLACK);
                    g.drawRect(x * CSIZEX, y * CSIZEY, CSIZEX, CSIZEY);
                }
            }

            for (int x = 0; x < satirX; x++) {
                for (int y = 0; y < sutunY; y++) {
                    if (map[x][y].getType() == 5 || map[x][y].getType() == 6) {
                        g.drawString("" + maliyet[x][y], (x * CSIZEX) + 5, (y * CSIZEY) + 10);
                    }

                }
            }
            for (int x = 0; x < satirX; x++) {
                for (int y = 0; y < sutunY; y++) {
                    g.setColor(Color.white);
                    if (map[x][y].getType() == 0) {
                        g.drawString("A", (x * CSIZEX) + 5, (y * CSIZEY) + 10);
                    }
                    if (map[x][y].getType() == 1) {
                        g.drawString("B", (x * CSIZEX) + 5, (y * CSIZEY) + 10);
                    }
                    if (map[x][y].getType() == 9) {
                        g.drawString("C", (x * CSIZEX) + 5, (y * CSIZEY) + 10);
                    }
                    if (map[x][y].getType() == 10) {
                        g.drawString("D", (x * CSIZEX) + 5, (y * CSIZEY) + 10);
                    }
                }
            }

        }

        @Override
        public void mouseDragged(MouseEvent e) {

        }

        @Override
        public void mouseMoved(MouseEvent e) {
        }

        @Override
        public void mouseClicked(MouseEvent e) {
        }

        @Override
        public void mouseEntered(MouseEvent e) {
        }

        @Override
        public void mouseExited(MouseEvent e) {
        }

        @Override
        public void mousePressed(MouseEvent e) {
            try {

                int x = e.getX() / CSIZEX;
                int y = e.getY() / CSIZEY;
                Node current = map[x][y];
                switch (araclar) {
                    case 0: {
                        if (current.getType() != 2) {
                            if (A_baslangicX > -1 && A_baslangicY > -1) {

                                map[A_baslangicX][A_baslangicY].setType(3);
                                map[A_baslangicX][A_baslangicY].setHops(-1);
                            }
                            current.setHops(0);
                            A_baslangicX = x;
                            A_baslangicY = y;
                            current.setType(0);
                        }
                        break;
                    }
                    case 1: {
                        if (current.getType() != 2) {
                            if (B_baslangicX > -1 && B_baslangicY > -1) {
                                map[B_baslangicX][B_baslangicY].setType(3);
                                map[B_baslangicX][B_baslangicY].setType(-1);
                            }
                            B_baslangicX = x;
                            B_baslangicY = y;
                            bx = x;
                            by = y;
                            current.setType(1);
                            current.setHops(0);
                        }
                        break;
                    }
                    case 3: {
                        cx = x;
                        cy = y;
                        current.setType(9);
                        current.setHops(0);
                        break;
                    }
                    case 4: {
                        dx = x;
                        dy = y;
                        current.setType(10);
                        current.setHops(0);
                        break;
                    }
                    default:
                        if (current.getType() != 0 && current.getType() != 1) {

                            current.setType(5);
                            altin_degerAta(); 

                        }

                        break;
                }
                Guncelle();
            } catch (Exception z) {
            }	
        }

        @Override
        public void mouseReleased(MouseEvent e) {
        }
    }

    class Algoritma {

        public void Dijkstra() {

            if (a == true) {
                A_oyuncu -= A_hedef_maliyet;
                harcanan_altin_A += A_hedef_maliyet;
                ArrayList<Node> priority = new ArrayList<Node>();
                A_ArrayList.add(map[A_baslangicX][A_baslangicY].x);
                A_ArrayList.add(map[A_baslangicX][A_baslangicY].y);
                priority.add(map[A_baslangicX][A_baslangicY]);

                while (devamMi) {
                    if (priority.size() <= 0) {
                        devamMi = false;
                        break;
                    }
                    int hops = priority.get(0).getHops() + 1;
                    ArrayList<Node> explored = komsu_kesfet(priority.get(0), hops);
                    if (explored.size() > 0) {
                        priority.remove(0);
                        priority.addAll(explored);
                        Guncelle();
                        delay();
                    } else {
                        priority.remove(0);
                    }
                }

            } else if (b == true) {
                ArrayList<Node> priority1 = new ArrayList<Node>();
               
                priority1.add(B_Sanal_Map[bx][by]);
               
                while (devamMi) {

                    if (priority1.size() <= 0) {
                        devamMi = false;
                        break;
                    }
                    int hops = priority1.get(0).getHops() + 1;
                    ArrayList<Node> explored1 = komsu_kesfet(priority1.get(0), hops);
                    if (explored1.size() > 0) {
                        priority1.remove(0);
                        priority1.addAll(explored1);
                        Guncelle();
                    } else {
                        priority1.remove(0);
                    }
                }
            } else if (c == true) {
                ArrayList<Node> priority2 = new ArrayList<Node>();
                
                priority2.add(C_Sanal_Map[cx][cy]);
                while (devamMi) {

                    if (priority2.size() <= 0) {
                        devamMi = false;
                        break;
                    }

                    int hops = priority2.get(0).getHops() + 1;
                    ArrayList<Node> explored2 = komsu_kesfet(priority2.get(0), hops);
                    if (explored2.size() > 0) {
                        priority2.remove(0);
                        priority2.addAll(explored2);
                        Guncelle();
               
                    } else {
                        priority2.remove(0);
                    }
                }
            } else if (d == true) {

                ArrayList<Node> priority3 = new ArrayList<Node>();
               
                priority3.add(D_Sanal_Map[dx][dy]);
                while (devamMi) {

                    if (priority3.size() <= 0) {
                        devamMi = false;
                        break;
                    }

                    int hops = priority3.get(0).getHops() + 1;
                    ArrayList<Node> explored3 = komsu_kesfet(priority3.get(0), hops);
                    if (explored3.size() > 0) {
                        priority3.remove(0);
                        priority3.addAll(explored3);
                        Guncelle();
                    } else {
                        priority3.remove(0);
                    }
                }
            }
            silcyan();
            Guncelle();
            delay();
        }

        public ArrayList<Node> komsu_kesfet(Node current, int hops) {

            if (a == true) {

                ArrayList<Node> kesfedildi = new ArrayList<Node>();
                for (int a = -1; a <= 1; a++) {
                    for (int b = -1; b <= 1; b++) {
                        int x_sınır = current.getX() + a;
                        int y_sınır = current.getY() + b;

                        if (abs(x_sınır - current.getX()) != 1 || abs(y_sınır - current.getY()) != 1) {

                            if ((x_sınır > -1 && x_sınır < satirX) && (y_sınır > -1 && y_sınır < sutunY)) {
                                Node komsu = map[x_sınır][y_sınır];
                                if ((komsu.getHops() == -1 || komsu.getHops() > hops)) {
                                    if (map[x_sınır][y_sınır].getType() == 5) {

                                        a_X = x_sınır;
                                        a_Y = y_sınır;
                                    }
                                    kesfet(komsu, current.getX(), current.getY(), hops);
                                    kesfedildi.add(komsu);
                                }
                            }
                        }
                    }
                }
                return kesfedildi;
            } else if (b == true) {

                ArrayList<Node> explored1 = new ArrayList<Node>();
                for (int a = -1; a <= 1; a++) {
                    for (int b = -1; b <= 1; b++) {
                        int xbound = current.getX() + a;
                        int ybound = current.getY() + b;

                        if (abs(xbound - current.getX()) != 1 || abs(ybound - current.getY()) != 1) {
                            if ((xbound > -1 && xbound < satirX) && (ybound > -1 && ybound < sutunY)) {
                                Node neighbor = B_Sanal_Map[xbound][ybound];
                                if ((neighbor.getHops() == -1 || neighbor.getHops() > hops)) {
                                    if (B_Sanal_Map[xbound][ybound].getType() == 5) {
                                        b_X = xbound;
                                        b_Y = ybound;
                                    }
                                    kesfet(neighbor, current.getX(), current.getY(), hops);
                                    explored1.add(neighbor);
                                }
                            }
                        }
                    }
                }
                return explored1;
            } else if (c == true) {

                ArrayList<Node> explored2 = new ArrayList<Node>();
                for (int a = -1; a <= 1; a++) {
                    for (int b = -1; b <= 1; b++) {
                        int xbound = current.getX() + a;
                        int ybound = current.getY() + b;

                        if (abs(xbound - current.getX()) != 1 || abs(ybound - current.getY()) != 1) {
                            if ((xbound > -1 && xbound < satirX) && (ybound > -1 && ybound < sutunY)) {
                                Node neighbor = C_Sanal_Map[xbound][ybound];
                                if ((neighbor.getHops() == -1 || neighbor.getHops() > hops)) {
                                    if (C_Sanal_Map[xbound][ybound].getType() == 5) {
                                        c_x = xbound;
                                        c_Y = ybound;
                                    }
                                    kesfet(neighbor, current.getX(), current.getY(), hops);
                                    explored2.add(neighbor);
                                }
                            }
                        }
                    }
                }
                return explored2;
            } else {

                ArrayList<Node> explored3 = new ArrayList<Node>();
               
                
                for (int a = -1; a <= 1; a++) {
                    for (int b = -1; b <= 1; b++) {
                        int xbound = current.getX() + a;
                        int ybound = current.getY() + b;

                        if (abs(xbound - current.getX()) != 1 || abs(ybound - current.getY()) != 1) {
                            if ((xbound > -1 && xbound < satirX) && (ybound > -1 && ybound < sutunY)) {
                                Node neighbor = D_Sanal_Map[xbound][ybound];

                                if ((neighbor.getHops() == -1 || neighbor.getHops() > hops)) {

                                    if (D_Sanal_Map[xbound][ybound].getType() == 5) {
                                        d_X = xbound;
                                        d_Y = ybound;

                                    }
                                    kesfet(neighbor, current.getX(), current.getY(), hops);
                                    explored3.add(neighbor);
                                }
                            }
                        }
                    }
                }
                return explored3;
            }
        }

        public void kesfet(Node current, int lastx, int lasty, int hops) {
            if (a == true) {
                if (current.getType() != 0 && current.getType() != 1 && current.getType() != 5 && current.getType() != 6 && current.getType() != 9 && current.getType() != 10) {
                    current.setType(4);
                }
                current.setLastNode(lastx, lasty);
                current.setHops(hops);

                if (current.getType() == 5) {
                    geri_takip(current.getLastX(), current.getLastY(), hops);
                    if (a_kontrol == 1) {

                    } else {
                        renk();
                    }
                }
            } else if (b == true) {
                if (current.getType() != 0 && current.getType() != 1 && current.getType() != 5 && current.getType() != 6 && current.getType() != 9 && current.getType() != 10) {

                    current.setType(4);
                }
                current.setLastNode(lastx, lasty);
                current.setHops(hops);

                if (current.getType() == 5) {
                    current.setType(3);
                    current.setHops(-1);
                    geri_takip(current.getLastX(), current.getLastY(), hops);
                }
            } else if (c == true) {
                if (current.getType() != 0 && current.getType() != 1 && current.getType() != 5 && current.getType() != 6 && current.getType() != 9 && current.getType() != 10) {
                    current.setType(4);
                }
                current.setLastNode(lastx, lasty);
                current.setHops(hops);

                if (current.getType() == 5) {
                    current.setType(3);
                    current.setHops(-1);
                    geri_takip(current.getLastX(), current.getLastY(), hops);
                }
            } else {
                if (current.getType() != 0 && current.getType() != 1 && current.getType() != 5 && current.getType() != 6 && current.getType() != 9 && current.getType() != 10) {

                       current.setType(4);
                }
                current.setLastNode(lastx, lasty);
                current.setHops(hops);

                if (current.getType() == 5) {
                    current.setType(3);
                    current.setHops(-1);
                    geri_takip(current.getLastX(), current.getLastY(), hops);
                }
            }
        }

        public void geri_takip(int lx, int ly, int hops) {
            if (a == true) {

                length = hops;
                A_altin_uzaklik = hops - 1;
                ileri = new next[hops];
                ileri[A_altin_uzaklik] = new next(a_X, a_Y);
                A_altin_uzaklik--;
                while (hops > 1) {
                    ileri[A_altin_uzaklik] = new next(lx, ly);
                    Node current = map[lx][ly];
                    lx = current.getLastX();
                    ly = current.getLastY();
                    hops--;
                    A_altin_uzaklik--;

                }
                kontrol = length;
                A_altin_uzaklik = length;
                devamMi = false;
            } else if (b == true) {
                length = hops;
                b_altina_uzaklik = hops - 1;
                ileri1 = new next[hops];
                ileri1[b_altina_uzaklik] = new next(b_X, b_Y);
                b_altina_uzaklik--;
                while (hops > 1) {
                    ileri1[b_altina_uzaklik] = new next(lx, ly);
                    Node current = B_Sanal_Map[lx][ly];
                    lx = current.getLastX();
                    ly = current.getLastY();
                    hops--;
                    b_altina_uzaklik--;

                }

                b_altina_uzaklik = length;
                devamMi = false;
            } else if (c == true) {
                length = hops;
                c_altina_uzaklik = hops - 1;
                ileri2 = new next[hops];
                ileri2[c_altina_uzaklik] = new next(c_x, c_Y);
                c_altina_uzaklik--;
                while (hops > 1) {
                    ileri2[c_altina_uzaklik] = new next(lx, ly);
                    Node current = C_Sanal_Map[lx][ly];
                    lx = current.getLastX();
                    ly = current.getLastY();
                    hops--;
                    c_altina_uzaklik--;

                }

                c_altina_uzaklik = length;
                devamMi = false;
            } else {
                length_D = hops;
               
                d_altina_uzaklik = hops - 1;
                ileri3 = new next[hops];
                ileri3[d_altina_uzaklik] = new next(d_X, d_Y);
                d_altina_uzaklik--;
                while (hops > 1) {
                    ileri3[d_altina_uzaklik] = new next(lx, ly);
                    Node current = D_Sanal_Map[lx][ly];
                    lx = current.getLastX();
                    ly = current.getLastY();
                    hops--;
                    d_altina_uzaklik--;

                }

                d_altina_uzaklik = length_D;
                devamMi = false;
            }
        }
    }

    public void silcyan() {
        if (a == true) {
            for (int i = 0; i < satirX; i++) {
                for (int j = 0; j < sutunY; j++) {
                    if (map[i][j].getType() == 4) {
                        map[i][j].setType(3);
                        map[i][j].setHops(-1);
                    }
                    if (map[i][j].getType() == 5) {
                        map[i][j].setHops(-1);
                    }
                }
            }
        } else if (b == true) {
            for (int i = 0; i < satirX; i++) {
                for (int j = 0; j < sutunY; j++) {
                    if (map[i][j].getType() == 4) {
                        map[i][j].setType(3);
                        map[i][j].setHops(-1);
                    }
                }
            }
        } else if (c == true) {
            for (int i = 0; i < satirX; i++) {
                for (int j = 0; j < sutunY; j++) {
                    if (map[i][j].getType() == 4) {
                        map[i][j].setType(3);
                        map[i][j].setHops(-1);
                    }
                }
            }
        }
    }

    public void altin_sayisi() {
        altin_sayisi = 0;
        for (int i = 0; i < satirX; i++) {
            for (int j = 0; j < sutunY; j++) {
                if (map[i][j].getType() == 5) {
                    altin_sayisi++;
                }
            }
        }
    }

    public void renk() {
        
        Node current = null;
        int i;
        if (a == true) {
            A_oyuncu -= A_hamle_maliyet;
            harcanan_altin_A += A_hamle_maliyet;
            map[A_baslangicX][A_baslangicY].setType(7);

            for (i = a_en_son_kalinan_adim; i < A_altin_uzaklik; i++) {
                current = map[ileri[i].x][ileri[i].y];
                A_ArrayList.add(current.x);
                A_ArrayList.add(current.y);

                if (current.getType() == 6) {
                    current.setType(5);
                    current.setHops(-1);
                } else if (current.getType() == 5) {
                    if (current.x == ileri[A_altin_uzaklik - 1].x && current.y == ileri[A_altin_uzaklik - 1].y) {
                        A_oyuncu += maliyet[current.x][current.y];
                        toplanan_altin_A += maliyet[current.x][current.y];
                        renkY_tut = 1;
                        current.setType(0);
                        current.setHops(0);
                        a_kontrol = 1;
                    }

                } else if (current.getType() == 1 || current.getType() == 9 || current.getType() == 10); else {
                    current.setType(7);
                    current.setHops(0);
                }
                if (adm == adim_sayisi) {
                    current.setType(0);
                    current.setHops(0);
                    break;
                }
                adm++;
                adima++;
            }
            if (adm == adim_sayisi && renkY_tut == 1) {
                renkY_tut = 2;
            }
            Guncelle();
            delay1(500);
            a_en_son_kalinan_adim = i;

        } else if (b == true) {
            map[bx][by].setType(8);
            B_oyuncu -= B_hamle_maliyet;
            harcanan_altin_B += B_hamle_maliyet;
            for (i = en_son_kal_adim_B; i < b_altina_uzaklik; i++) {

                current = map[yol[i].x][yol[i].y];
                B_ArrayList.add(current.x);
                B_ArrayList.add(current.y);
                if (current.getType() == 6) {
                    current.setType(5);
                    current.setHops(-1);
                } else if (current.getType() == 5) {
                    if (current.x == yol[b_altina_uzaklik - 1].x && current.y == yol[b_altina_uzaklik - 1].y) {
                      
                        B_oyuncu += maliyet[current.x][current.y];
                        toplanan_altin_B += maliyet[current.x][current.y];
                        map[current.x][current.y].setHops(-1);
                        map[current.x][current.y].setType(3);
                        renkK_tut = 1;
                        current.setType(1);
                        current.setHops(0);
                        b_kontrol = 1;
                        altin_sayisi();
                        if (altin_sayisi == 1) {
                            if (current.x == ileri[A_altin_uzaklik - 1].x && current.y == ileri[A_altin_uzaklik - 1].y) {
                                a_kontrol = 1;
                                a = true;
                                b = false;
                                a_en_son_kalinan_adim = 0;
                                abeyaztut = 0;
                                adm = 1;
                                adm2 = 1;
                                renkY_tut = 0;
                                Alg.Dijkstra();
                                a_kontrol = 0;
                                a = false;
                                b = true;

                            }
                        }

                    }

                } else if (current.getType() == 0 || current.getType() == 9 || current.getType() == 10); else {

                    current.setType(8);
                    current.setHops(0);
                }
                if (adm1 == adim_sayisi) {
                    current.setType(1);
                    current.setHops(0);
                    break;
                }
                adm1++;
                adimb++;
            }
            if (adm1 == adim_sayisi && renkK_tut == 1) {
                renkK_tut = 2;
            }
            Guncelle();
            delay1(500);
            en_son_kal_adim_B = i;
        } else if (c == true) {

            map[cx][cy].setType(8);
            C_oyuncu -= C_hamle_maliyet;
            harcanan_altin_C += C_hamle_maliyet;
            for (i = en_son_kal_adim_C; i < c_altina_uzaklik; i++) {

                current = map[yol_C[i].x][yol_C[i].y];
                C_ArrayList.add(current.x);
                C_ArrayList.add(current.y);
           
                if (current.getType() == 6) {
                    current.setType(5);
                    current.setHops(-1);
                } else if (current.getType() == 5) {
                    if (current.x == yol_C[c_altina_uzaklik - 1].x && current.y == yol_C[c_altina_uzaklik - 1].y) {
                     
                        C_oyuncu += maliyet[current.x][current.y];
                        toplanan_altin_C += maliyet[current.x][current.y];
                        c_kontrol = 1;
                        map[current.x][current.y].setHops(-1);
                        map[current.x][current.y].setType(3);
                        renkC_tut = 1;
                        current.setType(9);
                        current.setHops(0);
                        altin_sayisi();
                        if (altin_sayisi == 1) {
                            if (current.x == ileri[A_altin_uzaklik - 1].x && current.y == ileri[A_altin_uzaklik - 1].y) {
                                a_kontrol = 1;
                                a = true;
                                c = false;
                                a_en_son_kalinan_adim = 0;
                                abeyaztut = 0;
                                adm = 1;
                                adm2 = 1;
                                renkY_tut = 0;
                                Alg.Dijkstra();
                                a_kontrol = 0;
                                a = false;
                                c = true;

                            }
                            if (current.x == yol[b_altina_uzaklik - 1].x && current.y == yol[b_altina_uzaklik - 1].y) {
                                b_kontrol = 1;
                                b = true;
                                c = false;
                                adm1 = 1;
                                adm3 = 1;
                                en_son_kal_adim_B = 0;
                                bbeyaztut = 0;
                                renkK_tut = 0;
                                hesapla();
                                b_kontrol = 0;
                                b = false;
                                c = true;

                            }
                        }

                    }

                } else if (current.getType() == 0 || current.getType() == 1 || current.getType() == 10); else {

                    current.setType(8);
                    current.setHops(0);
                }
                if (adim_C == adim_sayisi) {
                    current.setType(9);
                    current.setHops(0);
                    break;
                }
                adim_C++;
                adimc++;
            }
            if (adim_C == adim_sayisi && renkC_tut == 1) {
                renkC_tut = 2;
            }
            Guncelle();
            delay1(500);
            en_son_kal_adim_C = i;
        } else if (d == true) {
            map[dx][dy].setType(8);
            D_oyuncu -= D_hamle_maliyet;
            harcanan_altin_D += D_hamle_maliyet;
            for (i = en_son_kal_adim_D; i < d_altina_uzaklik; i++) {

                current = map[ileri3[i].x][ileri3[i].y];
               
                D_ArrayList.add(current.x);
                D_ArrayList.add(current.y);
               
                if (current.getType() == 6) {

                    current.setType(5);
                    current.setHops(-1);
                } else if (current.getType() == 5) {
                    if (current.x == ileri3[d_altina_uzaklik - 1].x && current.y == ileri3[d_altina_uzaklik - 1].y) {
                        d_kontrol=1;
                        D_oyuncu += maliyet[current.x][current.y];
                        toplanan_altin_D += maliyet[current.x][current.y];
                      
                        map[current.x][current.y].setHops(-1);
                        map[current.x][current.y].setType(3);
                        renkD_tut = 1;
                        current.setType(10);
                        current.setHops(0);
                    }
                }
                else if (current.getType() == 0 || current.getType() == 1 || current.getType() == 9); else {

                    current.setType(8);
                    current.setHops(0);
                }
                if (adim_D == adim_sayisi) {
                    current.setType(10);
                    current.setHops(0);
                    break;
                }
                adim_D++;
                adimd++;
            }
            if (adim_D == adim_sayisi && renkD_tut == 1) {
                renkD_tut = 2;
            }
            Guncelle();
            delay1(500);
            en_son_kal_adim_D = i;
        }

        beyazYap();
    }

    public void beyazYap() {
        if (a == true) {
            int i;
            map[A_baslangicX][A_baslangicY].setType(3);
            Guncelle();
            delay1(500);
            for (i = abeyaztut; i < A_altin_uzaklik - 1; i++) {
                Node current = map[ileri[i].x][ileri[i].y];
                if (current.getType() == 5) {
                    current.setHops(-1);
                } else if (current.getType() == 1 || current.getType() == 9 || current.getType() == 10); else if (current.getType() == 0) {
                    current.setHops(0);
                } else {
                    current.setType(3);
                    current.setHops(-1);
                    Guncelle();
                    delay1(500);
                }
                if (adm2 == adim_sayisi) {
                    break;
                }

                adm2++;
            }
            abeyaztut = i;
            map[A_baslangicX][A_baslangicY].katman = -1;
            A_baslangicX = ileri[i].x;
            A_baslangicY = ileri[i].y;
            map[A_baslangicX][A_baslangicY].katman = 0;
            map[A_baslangicX][A_baslangicY].setType(0);
            i = 0;
            if (a_kontrol == 1) {
                a_en_son_kalinan_adim = 0;
                abeyaztut = 0;
                adm = 1;
                adm2 = 1;
                renkY_tut = 0;
                devamMi = true;
                kontrol();
                oyundaMi();
                if (a_oyun == true) {
                    Alg.Dijkstra();
                }
            }
            a_kontrol=0;
        } else if (b == true) {
            int i;
            map[bx][by].setType(3);
            map[bx][by].setHops(-1);
            Guncelle();
            delay1(500);
            for (i = bbeyaztut; i < b_altina_uzaklik - 1; i++) {
                Node current = map[yol[i].x][yol[i].y];
                if (current.getType() == 5) {
                    current.setHops(-1);
                } else if (current.getType() == 1) {
                    current.setHops(0);
                } else if (current.getType() == 0 || current.getType() == 9 || current.getType() == 10); else {
                    current.setType(3);
                    current.setHops(-1);
                    Guncelle();
                    delay1(500);
                }
                if (adm3 == adim_sayisi) {
                    break;
                }
                adm3++;
            }
            bx = yol[i].x;
            by = yol[i].y;
            map[bx][by].katman = 0;
            map[bx][by].setType(1);
            bbeyaztut = i;
            if (b_kontrol == 1) {
                adm1 = 1;
                adm3 = 1;
                en_son_kal_adim_B = 0;
                bbeyaztut = 0;
                renkK_tut = 0;
                kontrol();
                oyundaMi();
                if (b_oyun == true) {
                    hesapla();
                }

            }
            b_kontrol=0;
        } else if (c == true) {
            int i;

            map[cx][cy].setType(3);
            map[cx][cy].setHops(-1);
            Guncelle();
            delay1(500);
            for (i = cbeyaztut; i < c_altina_uzaklik - 1; i++) {
                Node current = map[yol_C[i].x][yol_C[i].y];
                if (current.getType() == 5) {
                    current.setHops(-1);
                } else if (current.getType() == 9) {
                    current.setHops(0);
                } else if (current.getType() == 0 || current.getType() == 1 || current.getType() == 10); else {
                    current.setType(3);
                    current.setHops(-1);
                    Guncelle();
                    delay1(500);
                }
                if (adim_C1 == adim_sayisi) {
                    break;
                }
                adim_C1++;
            }
            cx = yol_C[i].x;
            cy = yol_C[i].y;
            map[cx][cy].katman = 0;
            map[cx][cy].setType(9);
            cbeyaztut = i;

            if (c_kontrol == 1) {
                adim_C = 1;
                adim_C1 = 1;
                en_son_kal_adim_C = 0;
                cbeyaztut = 0;
                renkC_tut = 0;

                kontrol();
                oyundaMi();
                if (c_oyun == true) {
                    C_hesapla();
                }
            }
            c_kontrol=0;
        } else if (d == true) {
            
            int i;
            map[dx][dy].setType(3);
            map[dx][dy].setHops(-1);
            Guncelle();
            delay1(500);
            for (i = dbeyaztut; i < d_altina_uzaklik - 1; i++) {
                Node current = map[ileri3[i].x][ileri3[i].y];

                if (current.getType() == 5) {
                    current.setHops(-1);
                } else if (current.getType() == 10) {
                    current.setHops(0);
                } else if (current.getType() == 0 || current.getType() == 1 || current.getType() == 9); else {
                    current.setType(3);
                    current.setHops(-1);
                    Guncelle();
                    delay1(500);
                }

                if (adim_D1 == adim_sayisi) {
                    break;
                }
                adim_D1++;
            }
            dx = ileri3[i].x;
            dy = ileri3[i].y;
            map[dx][dy].katman = 0;
            map[dx][dy].setType(10);
            dbeyaztut = i;
          
            if (d_kontrol == 1) {
                adim_D = 1;
                adim_D1 = 1;
                en_son_kal_adim_D = 0;
                dbeyaztut = 0;
                renkD_tut = 0;
                kontrol();
                oyundaMi();
                if (d_oyun == true) {

                    hesapla_D();
                }
            }
            d_kontrol=0;
        }
    }

    class next {

        int x;
        int y;

        public next(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }

    public void oyundaMi() {
        if (a == true) {

            if (a_kontrol == 1 ) {
                kontrol();
                if (devamMi == false) {
                    a_oyun = false;
                }
                if (A_oyuncu - A_hamle_maliyet - A_hedef_maliyet < 0) {
                  
                    a_oyun = false;
                }
            } else {
                if (A_oyuncu - A_hamle_maliyet < 0) {
                  
                    a_oyun = false;
                }
            }

        } else if (b == true) {
          
            kontrol();
            if (devamMi == false) {
                b_oyun = false;
            }
            if (b_kontrol == 1 || oyuncu_sira==1) {
              
                if (B_oyuncu - B_hamle_maliyet - B_hedef_maliyet < 0) {
                 
                    b_oyun = false;
                }
            } else {
                if (B_oyuncu - B_hamle_maliyet < 0) {
                   
                    b_oyun = false;
                }
            }
        } else if (c == true) {
          
            kontrol();
            if (devamMi == false) {
                c_oyun = false;
            }
            if (c_kontrol == 1  || oyuncu_sira==2) {
                if (C_oyuncu - C_hamle_maliyet - C_hedef_maliyet < 0) {
                 
                    c_oyun = false;
                }
            } else {
                if (C_oyuncu - C_hamle_maliyet < 0) {
                   
                    c_oyun = false;
                }
            }
        } else if (d == true) {
          
            
            if (d_kontrol == 1  || oyuncu_sira==3) {
             
                if (D_oyuncu - D_hamle_maliyet - D_hedef_maliyet < 0) {
                   
                    d_oyun = false;
                }
            }
            else {
              
                if (D_oyuncu - D_hamle_maliyet < 0) {
                 
                    d_oyun = false;
                }
            }

        }
    }

    public void kontrol() {
        int d_bitis = 0;
        int temp_cntrl = 0;
        if (a == true || b == true || c == true) {

            for (int mp = 0; mp < satirX; mp++) {
                for (int i = 0; i < sutunY; i++) {
                    if (map[mp][i].getType() == 5) {
                        temp_cntrl = 1;
                        break;
                    } else {

                        devamMi = false;
                    }
                }
                if (temp_cntrl == 1) {
                    devamMi = true;
                    break;
                }

            }
        } else if (d == true) {
            int[] temp2 = new int[3];
            ArrayList<Integer> degisken = new ArrayList<>();
            int a_1 = 0, b_1 = 0, c_1 = 0, d_1, minimum = 0;
            int i1 = 0, j1 = 0;
          
            int deneme = 0, deneme2 = 0;
            int D_altin = 0;
            int temp = 0, temp3 = 0;
            uzunluk = new int[3];

            uzunluk[0] = abs(dx - ileri[A_altin_uzaklik - 1].x) + abs(dy - ileri[A_altin_uzaklik - 1].y);
            uzunluk[1] = abs(dx - yol[b_altina_uzaklik - 1].x) + abs(dy - yol[b_altina_uzaklik - 1].y);
            uzunluk[2] = abs(dx - yol_C[c_altina_uzaklik - 1].x) + abs(dy - yol_C[c_altina_uzaklik - 1].y);
            int min = uzunluk[0];
            for (int i = 0; i < satirX; i++) {
                for (int j = 0; j < sutunY; j++) {
                    if (map[i][j].getType() == 5) {
                        D_altin++;
                        i1 = i;
                        j1 = j;
                    }
                }
            }
            if (D_altin == 1) {
              
                temp2[0] = abs(A_baslangicX - map[i1][j1].x) + abs(A_baslangicY - map[i1][j1].y);
                temp2[1] = abs(bx - map[i1][j1].x) + abs(by - map[i1][j1].y);
                temp2[2] = abs(cx - map[i1][j1].x) + abs(cy - map[i1][j1].y);
                degisken.add(abs(A_baslangicX - map[i1][j1].x) + abs(A_baslangicY - map[i1][j1].y));
                degisken.add(abs(bx - map[i1][j1].x) + abs(by - map[i1][j1].y));
                degisken.add(abs(cx - map[i1][j1].x) + abs(cy - map[i1][j1].y));
                if (a_altin_kontrol == 1) {
                    degisken.remove(0);
                } else if (b_altin_kontrol == 1) {
                    degisken.remove(1);
                } else if (a_altin_kontrol == 1 && b_altin_kontrol == 1) {
                    degisken.remove(0);
                    degisken.remove(1);
                }
                minimum = temp2[0];
                for (int i = 0; i < 3; i++) {
                    if (temp2[i] < min) {

                        minimum = temp2[i];
                        temp3 = i;
                    }
                }
                d_1 = abs(dx - map[i1][j1].x) + abs(dy - map[i1][j1].y);
                if (d_1 < 4) {
                  
                    for (int i = 0; i < 3; i++) {
                        if (uzunluk[i] < min) {
                            min = uzunluk[i];
                            temp = i;
                        }
                    }
                    if (temp == 0) {
                    
                        birinci = 1;
                    } else if (temp == 1) {
                      
                        ikinci = 1;
                    } else if (temp == 2) {
                     
                        ucuncu = 1;
                    }
                } else if (d_1 < temp2[temp3]) {
                   
                    if (temp3 == 0) {
                    
                        birinci = 1;
                    } else if (temp3 == 1) {
                     
                        ikinci = 1;
                    } else if (temp3 == 2) {
                     
                        ucuncu = 1;
                    }
                } else {
                 
                    d_oyun = false;
                    d = false;
                }

            } else {
                int m_kontrol = 0;
                birinci = 0;
                ikinci = 0;
                ucuncu = 0;
                int kont_a, kont_b, kont_c;
               
                uzunluk_A = abs(A_baslangicX - ileri[A_altin_uzaklik - 1].x) + abs(A_baslangicY - ileri[A_altin_uzaklik - 1].y);
                uzunluk_B = abs(bx - yol[b_altina_uzaklik - 1].x) + abs(by - yol[b_altina_uzaklik - 1].y);
            
                uzunluk_C = abs(cx - yol_C[c_altina_uzaklik - 1].x) + abs(cy - yol_C[c_altina_uzaklik - 1].y);
                if (map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].getType() == 5) {
                    if (uzunluk_A < 4 && uzunluk[0] < 4) {
                    
                        birinci = 1;
                    } else if (uzunluk_A > 3 && (uzunluk[0] <= 6 && uzunluk[0] > 3)) {
                      
                        birinci = 1;
                    } else {
                        if (uzunluk[0] <= uzunluk_A) {
                         
                            if (ileri[A_altin_uzaklik - 1].x == yol[b_altina_uzaklik - 1].x && ileri[A_altin_uzaklik - 1].y == yol[b_altina_uzaklik - 1].y) {
                                kont_b = abs(bx - map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].x) + abs(by - map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].y);
                                if (uzunluk[0] >= kont_b) {
                                    deneme = 1;
                                } else {
                                    birinci = 1;
                                }
                            } else if (ileri[A_altin_uzaklik - 1].x == yol_C[c_altina_uzaklik - 1].x && ileri[A_altin_uzaklik - 1].y == yol_C[c_altina_uzaklik - 1].y) {
                                kont_c = abs(cx - map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].x) + abs(cy - map[ileri[A_altin_uzaklik - 1].x][ileri[A_altin_uzaklik - 1].y].y);
                                if (uzunluk[0] >= kont_c) {
                                    deneme = 1;
                                } else {
                                    birinci = 1;
                                }
                            } else {
                                birinci = 1;
                            }
                        } else {
                           
                            deneme = 1;
                        }
                    }
                }
                if (map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].getType() == 5) {
                    if (uzunluk_B < 4 && uzunluk[1] < 4) {
                      
                        ikinci = 1;
                    } else if (uzunluk_B > 3 && (uzunluk[1] <= 6 && uzunluk[1] > 3)) {
                      
                        ikinci = 1;
                    } else {
                        if (uzunluk[1] <= uzunluk_B) {
                            if (ileri[A_altin_uzaklik - 1].x == yol[b_altina_uzaklik - 1].x && ileri[A_altin_uzaklik - 1].y == yol[b_altina_uzaklik - 1].y) {
                                kont_a = abs(A_baslangicX - map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].x) + abs(A_baslangicY - map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].y);
                                if (uzunluk[1] >= kont_a) {
                                    deneme = 1;
                                } else {
                                    ikinci = 1;
                                }
                            } else if (yol[b_altina_uzaklik - 1].x == yol_C[c_altina_uzaklik - 1].x && yol[b_altina_uzaklik - 1].y == yol_C[c_altina_uzaklik - 1].y) {
                                kont_c = abs(cx - map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].x) + abs(cy - map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].y);
                                if (uzunluk[1] >= kont_c) {
                                    deneme = 1;
                                } else {
                                    ikinci = 1;
                                }
                            } else {
                                ikinci = 1;
                            }
                        } else {
                           
                            deneme = 1;
                        }

                    }
                }
                if (map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].getType() == 5) {
                    if (uzunluk_C < 4 && uzunluk[2] < 4) {
          
                        ucuncu = 1;
                    } else if (uzunluk_C > 3 && (uzunluk[2] <= 6 && uzunluk[2] > 3)) {
                       
                        ucuncu = 1;
                    } else {
                        if (uzunluk[2] <= uzunluk_C) {
                            if (ileri[A_altin_uzaklik - 1].x == yol_C[c_altina_uzaklik - 1].x && ileri[A_altin_uzaklik - 1].y == yol_C[c_altina_uzaklik - 1].y) {
                                kont_a = abs(A_baslangicX - map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].x) + abs(A_baslangicY - map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].y);
                                if (uzunluk[2] >= kont_a) {
                                    deneme = 1;
                                } else {
                                    ucuncu = 1;
                                }
                            } else if (yol[b_altina_uzaklik - 1].x == yol_C[c_altina_uzaklik - 1].x && yol[b_altina_uzaklik - 1].y == yol_C[c_altina_uzaklik - 1].y) {
                                kont_b = abs(bx - map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].x) + abs(by - map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].y);
                                if (uzunluk[2] >= kont_b) {
                                    deneme = 1;
                                } else {
                                    ucuncu = 1;
                                }

                            } else {
                                ucuncu = 1;
                            }
                        } else {
                           
                            deneme = 1;
                        }
                    }
                }

                if (birinci == 0 || ikinci == 0 || ucuncu == 0) {

                    deneme = 1;
                }
                if (birinci != 0 || ikinci != 0 || ucuncu != 0) {

                    deneme = 0;
                }

                if (deneme == 1) {

                    temp_cntrl = 0;
                    D_Map = new Node[satirX][sutunY];
                    for (int mn = 0; mn < satirX; mn++) {
                        for (int j = 0; j < sutunY; j++) {
                            D_Map[mn][j] = new Node(3, map[mn][j].getX(), map[mn][j].getY());
                            D_Map[mn][j].setHops(-1);

                            if ((mn == ileri[A_altin_uzaklik - 1].x && ileri[A_altin_uzaklik - 1].y == j) || (mn == yol[b_altina_uzaklik - 1].x && yol[b_altina_uzaklik - 1].y == j) || (mn == yol_C[c_altina_uzaklik - 1].x && yol_C[c_altina_uzaklik - 1].y == j)) {

                            } else {
                                D_Map[mn][j] = new Node(map[mn][j].getType(), map[mn][j].getX(), map[mn][j].getY());
                                D_Map[mn][j].setHops(map[mn][j].getHops());
                            }

                        }
                    }
                    for (int mp = 0; mp < satirX; mp++) {
                        for (int i = 0; i < sutunY; i++) {
                            if (D_Map[mp][i].getType() == 5) {
                                temp_cntrl = 1;
                                break;
                            } else {

                                d_oyun = false;
                                d = false;
                            }
                        }
                        if (temp_cntrl == 1) {
                            d_oyun = true;
                            d = true;
                            devamMi = true;
                            break;
                        }

                    }
                }
            }

        }

    }

    public void deger() {
        if (oyuncu_sira % 4 == 0) {
            b = false;
            c = false;
            d = false;
            a = true;
            oyundaMi();
            a = false;
            if (a_oyun == true) {

                a = true;
                b = false;
                c = false;
                d = false;
                kontrol();
            } else {
                a = false;
                b = false;
                c = false;
                d = false;
            }
        }
        if (oyuncu_sira % 4 == 1) {
            a = false;
            c = false;
            d = false;
            b = true;
            oyundaMi();
            b = false;
            if (b_oyun == true) {

                a = false;
                b = true;
                c = false;
                d = false;
                kontrol();
            } else {
                a = false;
                b = false;
                c = false;
                d = false;
            }
        }
     
        if (oyuncu_sira % 4 == 2) {
           
            a = false;
            b = false;
            d = false;
            c = true;
            oyundaMi();
            c = false;
            if (c_oyun == true) {

                a = false;
                b = false;
                c = true;
                d = false;
                kontrol();
            } else {
                a = false;
                b = false;
                c = false;
                d = false;
            }
        }
        if (oyuncu_sira % 4 == 3) {
        
            a = false;
            b = false;
            c = false;
            d = true;
            oyundaMi();
            d = false;
            if (d_oyun == true) {

                a = false;
                b = false;
                c = false;
                d = true;
               
                kontrol();
            } else {
                a = false;
                b = false;
                c = false;
                d = false;
            }
        }

        oyuncu_sira++;

     
        if (a_oyun == false && b_oyun == false && c_oyun == false && d_oyun == false || devamMi == false) {
            boolean yaz = true, yaz1 = true, yaz2 = true, yaz3 = true;
            int sayac = 0;
            int j = 0;
            try {
                writer.write("A                                                  B                                              C                                               D\n");

                while (true) {
                    if (yaz == true) {

                        if (sayac == A_ArrayList.size()) {
                            yaz = false;
                            writer.write("                                              ");
                        } else {
                            writer.write("[X: " + A_ArrayList.get(j) + "Y: " + A_ArrayList.get(j + 1) + "]                                    ");
                        }
                    } else {
                        writer.write("                                              ");
                    }
                    if (yaz1 == true) {

                        if (sayac == B_ArrayList.size()) {
                            yaz1 = false;
                            writer.write("                                              ");
                        } else {
                            writer.write("[X: " + B_ArrayList.get(j) + "Y: " + B_ArrayList.get(j + 1) + "]                                    ");
                        }
                    } else {
                        writer.write("                                              ");
                    }
                    if (yaz2 == true) {

                        if (sayac == C_ArrayList.size()) {
                            yaz2 = false;
                            writer.write("                                    ");
                        } else {
                            writer.write("[X: " + C_ArrayList.get(j) + "Y: " + C_ArrayList.get(j + 1) + "]                                    ");
                        }
                    } else {
                        writer.write("                                    ");
                    }
                    if (yaz3 == true) {

                        if (sayac == D_ArrayList.size()) {
                            yaz3 = false;
                            writer.write("                                    ");
                        } else {
                            writer.write("[X: " + D_ArrayList.get(j) + "Y: " + D_ArrayList.get(j + 1) + "]");
                        }
                    } else {
                        writer.write("                                    ");
                    }
                    writer.write("\n\n");
                    j += 2;
                    sayac += 2;
                    if (yaz == false && yaz1 == false && yaz2 == false && yaz3 == false) {
                        break;
                    }
                }

                tablo.setVisible(true);
                ozet_Tablo();

                writer.close();
            } catch (IOException ex) {
                Logger.getLogger(Altin_Oyunu.class.getName()).log(Level.SEVERE, null, ex);
            }

            devamMi = false;
        } else {
            devamMi = true;
        }
        if (devamMi) {
            if (a == true) {
              
                if (A_altin_uzaklik == a_en_son_kalinan_adim || A_altin_uzaklik == 0 || renkY_tut == 2) {
                  
                    a_en_son_kalinan_adim = 0;
                    abeyaztut = 0;
                    adm = 1;
                    adm2 = 1;
                    renkY_tut = 0;
        
                    Alg.Dijkstra();

                } else {

                    int deneme = 0;
                    for (int kntrl = 0; kntrl < kontrol; kntrl++) {
                        if (map[ileri[kontrol - 1].x][ileri[kontrol - 1].y].getType() != 5 /*|| map[ileri[kntrl].x][ileri[kntrl].y].getType() == 1 || map[ileri[kntrl].x][ileri[kntrl].y].getType() == 9 || map[ileri[kntrl].x][ileri[kntrl].y].getType() == 10*/) {
                           a_kontrol=1;
                            deneme = 1;
                        }
                    }
                    if (deneme == 1) {
                        a_en_son_kalinan_adim = 0;
                        abeyaztut = 0;
                        adm = 1;
                        adm2 = 1;
                        renkY_tut = 0;
                        a_kontrol=1;
                        oyundaMi();
                        a_kontrol=0;
                        Alg.Dijkstra();
                    } else {
                        if (a_en_son_kalinan_adim == 0) {
                            adm = 1;
                            adm2 = 1;
                        } else {
                            adm = 0;
                            adm2 = 0;
                        }

                        renk();
                    }

                }
            } else if (b == true) {

                if (b_altina_uzaklik == en_son_kal_adim_B || b_altina_uzaklik == 0 || renkK_tut == 2) {

                
                    adm1 = 1;
                    adm3 = 1;
                    en_son_kal_adim_B = 0;
                    bbeyaztut = 0;
                    renkK_tut = 0;
                    hesapla();
                    renk();
                } else {
                    int deneme = 0;
                    for (int kntrl = en_son_kal_adim_B; kntrl < b_altina_uzaklik; kntrl++) {
                        if (map[yol[b_altina_uzaklik - 1].x][yol[b_altina_uzaklik - 1].y].getType() != 5/* || map[yol[kntrl].x][yol[kntrl].y].getType() == 0 || map[yol[kntrl].x][yol[kntrl].y].getType() == 9 || map[yol[kntrl].x][yol[kntrl].y].getType() == 10*/) {
                            deneme = 1;
                             b_kontrol=1;
                        }
                    }
                    if (deneme == 1) {
                     
                        adm1 = 1;
                        adm3 = 1;
                        en_son_kal_adim_B = 0;
                        bbeyaztut = 0;
                        renkK_tut = 0;
                       
                        oyundaMi();
                        b_kontrol=0;
                        hesapla();
                        renk();
                    } else {
                      
                        if (en_son_kal_adim_B == 0) {
                            adm1 = 1;
                            adm3 = 1;
                        } else {
                            adm1 = 0;
                            adm3 = 0;
                        }
                        renk();
                    }

                }

            } else if (c == true) {
                if (c_altina_uzaklik == en_son_kal_adim_C || c_altina_uzaklik == 0 || renkC_tut == 2) {
                    adim_C = 1;
                    adim_C1 = 1;
                    en_son_kal_adim_C = 0;
                    cbeyaztut = 0;
                    renkC_tut = 0;
                    C_hesapla();
                    renk();
                } else {
                    int deneme = 0;
                    for (int kntrl = en_son_kal_adim_C; kntrl < c_altina_uzaklik; kntrl++) {
                        if (map[yol_C[c_altina_uzaklik - 1].x][yol_C[c_altina_uzaklik - 1].y].getType() != 5 /*|| map[yol_C[kntrl].x][yol_C[kntrl].y].getType() == 0 || map[yol_C[kntrl].x][yol_C[kntrl].y].getType() == 1 || map[yol_C[kntrl].x][yol_C[kntrl].y].getType() == 10*/) {
                            deneme = 1;
                            c_kontrol=1;
                        }
                    }
                    if (deneme == 1) {


                        adim_C = 1;
                        adim_C1 = 1;
                        en_son_kal_adim_C = 0;
                        cbeyaztut = 0;
                        renkC_tut = 0;
oyundaMi();
c_kontrol=0;
                        C_hesapla();
                        renk();
                    } else {

                      
                        if (en_son_kal_adim_C == 0) {
                            adim_C = 1;
                            adim_C1 = 1;
                        } else {
                            adim_C = 0;
                            adim_C1 = 0;
                        }
                        renk();
                    }

                }
            } else if (d == true) {

                if (d_altina_uzaklik == en_son_kal_adim_D || d_altina_uzaklik == 0 || renkD_tut == 2) {
                   
                    adim_D = 1;
                    adim_D1 = 1;
                    en_son_kal_adim_D = 0;
                    dbeyaztut = 0;
                    renkD_tut = 0;
                    hesapla_D();
                    renk();
                } else {
                    int deneme = 0;
                    for (int kntrl = 0; kntrl < d_altina_uzaklik; kntrl++) {
                        if (map[ileri3[d_altina_uzaklik - 1].x][ileri3[d_altina_uzaklik - 1].y].getType() != 5/* || map[ileri3[kntrl].x][ileri3[kntrl].y].getType() == 0 || map[ileri3[kntrl].x][ileri3[kntrl].y].getType() == 1 || map[ileri3[kntrl].x][ileri3[kntrl].y].getType() == 9*/) {
                            deneme = 1;
                            d_kontrol=1;
                        }
                    }
                    if (deneme == 1) {

                       oyundaMi();
                       d_kontrol=0;
                        adim_D = 1;
                        adim_D1 = 1;
                        en_son_kal_adim_D = 0;
                        dbeyaztut = 0;
                        renkD_tut = 0;
                        hesapla_D();
                        renk();
                    } else {

               
                        if (en_son_kal_adim_D == 0) {

                            adim_D = 1;
                            adim_D1 = 1;
                        } else {

                            adim_D = 0;
                            adim_D1 = 0;
                        }
                        renk();
                    }

                }
            }
            deger();
        }

    }

    class Node {

        private int hucre_renk = 0;
        private int katman;
        private int x;
        private int y;
        private int lastX;
        private int lastY;
        private double dToEnd = 0;

        public Node(int type, int x, int y) {

            hucre_renk = type;
            this.x = x;
            this.y = y;
            katman = -1;
        }

        public int getX() {
            return x;
        }		

        public int getY() {
            return y;
        }

        public int getLastX() {
            return lastX;
        }

        public int getLastY() {
            return lastY;
        }

        public int getType() {
            return hucre_renk;
        }

        public int getHops() {
            return katman;
        }

        public void setType(int type) {
            hucre_renk = type;
        }		
        public void setLastNode(int x, int y) {
            lastX = x;
            lastY = y;
        }

        public void setHops(int hops) {
            this.katman = hops;
        }
    }

}
