# translate

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Text;
using System.Text.RegularExpressions;
using System.Threading;
using System.Threading.Tasks;
using System.Web;
using System.Windows.Forms;
using Microsoft.VisualBasic;
namespace English
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

      
        public string teleffuz;
        private void button1_Click_1(object sender, EventArgs e)
        {

                
                    try
                    {
                        var url = new Uri("https://www.okunusu.com/" + txtArananKelime.Text + "-okunusu/"); // url oluştruduk
                        var client = new WebClient(); // siteye erişim için client tanımladık
                        var html = client.DownloadString(url); //sitenin html lini indirdik
                        HtmlAgilityPack.HtmlDocument doc = new HtmlAgilityPack.HtmlDocument(); //burada HtmlAgilityPack Kütüphanesini kullandık
                        doc.LoadHtml(html); // indirdiğimiz sitenin html lini oluşturduğumuz dokumana dolduruyoruz
                        var veri1 = doc.DocumentNode.SelectNodes("/html[1]/body[1]/div[3]/div[1]/div[1]/div[1]/div[1]/p[4]/font[1]")[0];
                        var veri2 = doc.DocumentNode.SelectNodes("html[1]/body[1]/div[3]/div[1]/div[1]/div[1]/div[1]/p[2]")[0];
                        if (veri1 != null && veri2 != null)
                        {

                            txtCikanSonuc.Text = txtArananKelime.Text + "(" + veri1.InnerHtml + ")=" + Strings.Mid(veri2.InnerHtml, txtArananKelime.TextLength + 10);
                            teleffuz= "(" + veri1.InnerHtml + ")";

                        }
                    }
                    catch (Exception)
                    {

                        MessageBox.Show("Arama sonucu bulunamadı veya Hatalı kelime girildi!");
                         return;
                     }
              
            try
            {


                //http://vipingilizce.net/kelime/say/?tumunu_goster=ok#diger

                var url = new Uri("http://vipingilizce.net/kelime/" + txtArananKelime.Text + "/"); // url oluştruduk
                var client = new WebClient(); // siteye erişim için client tanımladık
                var html = client.DownloadString(url); //sitenin html lini indirdik
                HtmlAgilityPack.HtmlDocument doc = new HtmlAgilityPack.HtmlDocument(); //burada HtmlAgilityPack Kütüphanesini kullandık
                doc.LoadHtml(html); // indirdiğimiz sitenin html lini oluşturduğumuz dokumana dolduruyoruz
                var veri1 = doc.DocumentNode.SelectNodes("//body/div[1]/div[1]/div[1]/div[1]/p[1]")[0];
                if (veri1 != null)
                {

                    string[] sayilar = Regex.Split(veri1.InnerHtml, @"\D+");
                    foreach (string s in sayilar)
                    {
                        int sayi;
                        if (int.TryParse(s, out sayi))
                        {
                            CümleSayisi = s;
                        }
                    }


                }
            }
            catch { }



            txtKelimeKullanimi.Clear();
            String Cumle1, Cumle2, Cumle3, Cumle4, Cumle5, Cumle6, Cumle7, Cumle8, Cumle9, Cumle10;
            string Kelime1 = "<br>", Kelime2 = "<b>", Kelime3 = "</b>", Kelime4 = "<em>", Kelime5 = "</em>", Kelime6 = txtArananKelime.Text;
            string Gecici1, Gecici2, Gecici3, Gecici4, Gecici5, Gecici6;
            try
            {
                //http://vipingilizce.net/kelime/sky/?tumunu_goster=ok#diger
                var url = new Uri("http://vipingilizce.net/kelime/" + txtArananKelime.Text + "/?tumunu_goster=ok#diger"); // url oluştruduk
                var client = new WebClient(); // siteye erişim için client tanımladık
                var html = client.DownloadString(url); //sitenin html lini indirdik
                HtmlAgilityPack.HtmlDocument doc = new HtmlAgilityPack.HtmlDocument(); //burada HtmlAgilityPack Kütüphanesini kullandık
                doc.LoadHtml(html); // indirdiğimiz sitenin html lini oluşturduğumuz dokumana dolduruyoruz
                txtKelimeKullanimi.AppendText("――――――――――――――――――――――――――――――――――――――――" + "\r\n");
                for (int i = 1; i <= Convert.ToInt32(CümleSayisi); i++)
                {
                    var veri1 = doc.DocumentNode.SelectNodes("//body[1]/div[1]/div[1]/div[1]/div[1]/div[3]/div[1]/div[3]/p[" + i + "]")[0];
                    if (veri1 != null)
                    {
                      
                        Cumle1 = veri1.InnerHtml;
                        Gecici1 = Cumle1.Replace(Kelime1, "\r\n" + "➦ ");
                        Gecici2 = Gecici1.Replace(Kelime2, "");
                        Gecici3 = Gecici2.Replace(Kelime3, "");
                        Gecici4 = Gecici3.Replace(Kelime4, "");
                        Gecici5 = Gecici4.Replace(Kelime5, "");
                        Gecici6 = Gecici5.Replace(Kelime6, txtArananKelime.Text + teleffuz);

                        //teleffuz
                        txtKelimeKullanimi.AppendText("➦ " + Gecici6 + "\r\n");
                        txtKelimeKullanimi.AppendText("――――――――――――――――――――――――――――――――――――――――" + "\r\n");
                    }

                }

            }
            catch { }
       
            boya(txtArananKelime.Text+teleffuz, Color.Red, false);
        }

        private void button2_Click(object sender, EventArgs e)
        {
            Thread thread = new Thread(() => Clipboard.SetText(txtCikanSonuc.Text.ToString()));
            thread.SetApartmentState(ApartmentState.STA);
            thread.Start();
            thread.Join();
        }
        public void boya(string kelime, Color renk, Boolean tamam)
        {//teleffuz
            int textEnd = txtKelimeKullanimi.TextLength;
            int index = 0;
            int lastIndex = txtKelimeKullanimi.Text.LastIndexOf(kelime);

            while (index < lastIndex)
            {
                if (tamam)
                {
                    txtKelimeKullanimi.Find(kelime, index, textEnd, RichTextBoxFinds.WholeWord);
                }
                else
                {
                    txtKelimeKullanimi.Find(kelime, index, textEnd, RichTextBoxFinds.None);
                }

                txtKelimeKullanimi.SelectionBackColor = renk;
                index = txtKelimeKullanimi.Text.IndexOf(kelime , index) + 1;
            }
        }
        int Konum = -1;
        public string CümleSayisi;
        public string CümleSayisi2;
        private void button3_Click_1(object sender, EventArgs e)
        {
            txtArananKelime.Clear();
            txtCikanSonuc.Clear();
            txtKelimeKullanimi.Clear();
        }

        private void button4_Click_2(object sender, EventArgs e)
        {
            
                    try
                    {
                        var url = new Uri("https://www.okunusu.com/" + txtArananKelime.Text + "-okunusu/"); // url oluştruduk
                        var client = new WebClient(); // siteye erişim için client tanımladık
                        var html = client.DownloadString(url); //sitenin html lini indirdik
                        HtmlAgilityPack.HtmlDocument doc = new HtmlAgilityPack.HtmlDocument(); //burada HtmlAgilityPack Kütüphanesini kullandık
                        doc.LoadHtml(html); // indirdiğimiz sitenin html lini oluşturduğumuz dokumana dolduruyoruz
                        var veri1 = doc.DocumentNode.SelectNodes("/html[1]/body[1]/div[3]/div[1]/div[1]/div[1]/div[1]/p[4]/font[1]")[0];
                        var veri2 = doc.DocumentNode.SelectNodes("html[1]/body[1]/div[3]/div[1]/div[1]/div[1]/div[1]/p[2]")[0];
                        if (veri1 != null && veri2 != null)
                        {

                            txtCikanSonuc.Text = txtArananKelime.Text + "(" + veri1.InnerHtml + ")=" + Strings.Mid(veri2.InnerHtml, txtArananKelime.TextLength + 10);

                            Thread thread = new Thread(() => Clipboard.SetText(txtCikanSonuc.Text.ToString()));
                            thread.SetApartmentState(ApartmentState.STA);
                            thread.Start();
                            thread.Join();

                        }
                    }
                    catch (Exception)
                    {

                        MessageBox.Show("Arama sonucu bulunamadı veya Hatalı kelime girildi!");
                        return;
                    }
        }
        public String TranslateEnglish(String word)
        {
            var toLanguage = "tr";//English
            var fromLanguage = "en";//Deutsch
            var url = $"https://translate.googleapis.com/translate_a/single?client=gtx&sl={fromLanguage}&tl={toLanguage}&dt=t&q={System.Web.HttpUtility.UrlEncode(word)}";
            var webClient = new WebClient
            {
                Encoding = System.Text.Encoding.UTF8
            };
            var result = webClient.DownloadString(url);
            try
            {
                result = result.Substring(4, result.IndexOf("\"", 4, StringComparison.Ordinal) - 4);
                return result;
            }
            catch
            {
                return "Error";
            }
        }
        public String TranslateTurkish(String word)
        {
            var toLanguage = "en";//English
            var fromLanguage = "tr";//Deutsch
            var url = $"https://translate.googleapis.com/translate_a/single?client=gtx&sl={fromLanguage}&tl={toLanguage}&dt=t&q={System.Web.HttpUtility.UrlEncode(word)}";
            var webClient = new WebClient
            {
                Encoding = System.Text.Encoding.UTF8
            };
            var result = webClient.DownloadString(url);
            try
            {
                result = result.Substring(4, result.IndexOf("\"", 4, StringComparison.Ordinal) - 4);
                return result;
            }
            catch
            {
                return "Error";
            }
        }
        private void button7_Click(object sender, EventArgs e)
        {
            txtTurkish.Text = TranslateEnglish(txtEnglish.Text);
        }

        private void button6_Click(object sender, EventArgs e)
        {
            txtEnglish.Text = TranslateTurkish(txtTurkish.Text);
        }

    }
}

