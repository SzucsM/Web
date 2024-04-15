# Web

<form id="form1" runat="server" action="feldolgozas.aspx" method="post">
   
        Aru neve: <input type="text" name="nev" /><br />
        Ar: <input type="number" name="ar" /><br />
        Kategoria: <input type="text" name="kategoria" /><br />    
        Raktaron: <input type="text" name="raktaron" /><br />
        <input type ="submit" value="kuldes"/>

    </form>
///////////////////
 <asp:GridView ID="GridView1" runat="server" AutoGenerateColumns="false">
            <columns>
                 <asp:Boundfield Datafield="Kategoria" HeaderText="Kategoria"/>
                <asp:Boundfield Datafield="Elnevezes" HeaderText="Az etel neve"/>
                <asp:Boundfield Datafield="Raktaron" HeaderText="Keszleten"/>
                <asp:Boundfield Datafield="Ar" HeaderText="Ar"/>
            </columns>
            <EmptyDataTemplate>
                    Nincs semmi!

            </EmptyDataTemplate>
        </asp:GridView>
////////////////
  public class aruk {
            private string elnevezes;
            private string ar;
            private string kategoria;
            private string raktaron;

            public string Elnevezes
            {
                get { return elnevezes; }
                set { elnevezes = value; }
            }
            public string Ar
            {
                get { return ar; }
                set { ar = value; }
            }
            public string Kategoria
            {
                get { return kategoria; }
                set { kategoria = value; }
            }
         
            public string Raktaron
            {
                get { return raktaron; }
                set { raktaron = value; }
            }
           


        }
/////////////////////////
 try
            {
                string kapottnev;
                string kapottar;
                string kapottraktaron;
                string kapottkategoria;

                if (Request.Form["nev"] != null)
                {
                    kapottnev = Request.Form["nev"];
                    Response.Write("Nev: " + kapottnev + " | ");

                }
                if (Request.Form["ar"] != null)
                {
                    kapottar = Request.Form["ar"];
                    Response.Write("Ar: " + kapottar + " | ");

                }
                if (Request.Form["kategoria"] != null)
                {
                    kapottkategoria = Request.Form["kategoria"];
                    Response.Write("Kategoria: " + kapottkategoria + " | ");

                }
                if (Request.Form["raktaron"] != null)
                {
                    kapottraktaron = Request.Form["raktaron"];
                    Response.Write("Keszlet: " + kapottraktaron);

                }
                string utvonal = Server.MapPath("~/arucikkek.txt");
                StreamWriter sw = new StreamWriter(utvonal);


                sw.Write(Request.Form["nev"] + '|' + Request.Form["ar"] + '|' + Request.Form["raktaron"] + '|' + Request.Form["kategoria"]);
               

                sw.Close();


                List<aruk> lista = new List<aruk>();
                string sor;
                string utvonal2 = Server.MapPath("~/arucikkek.txt");
                StreamReader sr = new StreamReader(utvonal);

                while ((sor = sr.ReadLine()) != null)
                {
                    aruk item = new aruk();
                    string[] line = sor.ToString().Split('|');

                    if (line.Length < 2)
                    {
                        break;

                    }
                    item.Kategoria = line[3].Trim();
                    item.Elnevezes = line[0].Trim();
                    item.Raktaron = line[2].Trim();
                    item.Ar = line[1].Trim();
                    lista.Add(item);

                }
                sr.Close();

                GridView1.DataSource = lista;
                GridView1.DataBind();
            }
            catch (Exception error)
            {
                Response.Write("Hiba" + error.Message);
            }
            
