using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace ConsoleApplication24
{
    class Program
    {
        static void Main(string[] args)
        {
            node n = new node();
            tree a = new tree();
            //a.add_node(4);
            //a.add_node(7);
            //a.add_node(12);
            //a.add_node(15);
            //a.add_node(3);
            //a.add_node(5);
            //a.add_node(14);
            //a.add_node(18);
            //a.add_node(16);
            //a.add_node(17);
            //List<double> b = new List<double>();
            //a.ConvertTreeToList(a, b);
            // List<double> c = new List<double>();
            //double[] b =new double[] { };
            //string str;
            double[] arr = new double[5];
            using (StreamReader sr = new StreamReader("C:\\Users\\HP-15\\Desktop\\redblack.txt"))
            {
                for (int i = 0; i < 5; i++)
                {
                    arr[i] = Convert.ToDouble(sr.ReadLine());
                    a.add_node(arr[i]);
                }
                
            }
            //for (int i = 0; i < str.Length; i++)
            //{


                //Convert.ToDouble(str[i]);
                // b[]
            //    b[i] = double.Parse(str[i]);
            //}
            //Console.WriteLine("Red Black tree");
            //Console.WriteLine("The input is :");

            //Console.WriteLine();
            ////a.inorder();
            //for (int i = 0; i < b.Length ; i++)
            //{
            //    Console.WriteLine(b[i]);
            //}
            //start:
            //Console.WriteLine("1. Add\n2. Delete\n3. Find\n4. Traverse");
            //char s = Console.ReadKey().KeyChar;
            //switch(s)
            //{
            //    case '1':
            //        double ff =Convert.ToDouble(Console.ReadLine());
            //        a.add_node(ff);
            //        break;
            //    case '2':
            //        a.deletionRBT(Convert.ToDouble(Console.ReadLine()));
            //        break;
            //    case '3':
            //        a.Findnum(Convert.ToDouble(Console.ReadLine()));
            //        break;
            //    case '4':
            //        Console.WriteLine();
            //        a.inorder();
            //        break;
            //    default:
            //        goto start;
            //        break;
            //}
            //a.add_node(1);
            //a.add_node(2);
            //a.add_node(3);
            //a.add_node(4);
            //a.add_node(5);

            Console.WriteLine("\ninorder");
            a.inorder();
            
            //a.deletionRBT(16);
            //Console.WriteLine();
            //a.inorder();
            
            Console.ReadLine();
        }
    }
    class node
    {
        public double value;
        public node left;
        public node right;
        public string color;
        public node()
        {
            left = null;
            right = null;
            color = "red";
        }
    }

    class tree
    {
        public int count = 0;
        node head;
        node current;
        double current_x;

        public tree()
        {
            head = new node();
            current = head;
        }
        public void add_node(double x)
        {
            if (count == 0)
            {
                head.value = x;
                head.color = "black";
                current = head;
                count++;
            }
            else
            {
                node n = new node();
                n.value = x;
                current = head;

                while ((current.right != null) || (current.left != null))
                {
                    if (n.value >= current.value)
                    {
                        if (current.value == n.value)
                        {
                            break;
                        }
                        if (current.right == null)
                        {
                            break;
                        }
                        current = current.right;
                    }
                    else
                    {
                        if (current.value == n.value)
                        {
                            break;
                        }
                        if (current.left == null)
                        {
                            break;
                        }
                        current = current.left;
                    }

                }
                if (n.value > current.value)
                {
                    current.right = n;
                    count++;
                }
                else if (n.value < current.value)
                {
                    current.left = n;
                    count++;
                }
                ///////////////////////

                balancing(x, n);
                head.color = "black";
                removenil();
                //List<double> a = new List<double>();
                //node t = head;
                //ConvertTreeToList(t, a);
                //using (StreamWriter sr = new StreamWriter(@"\\C:\\Users\\HP-15\\Desktop\\redblack.txt"))
                //{
                //    foreach (double aaa in a)
                //    {
                //        sr.WriteLine(aaa);
                //    }
                //}
            }
        }
        public void balancing(double x, node n)
        {

            current_x = x;

            while (n != null)
            {
                node par = search_parent(current_x, n);
                if (par == null) { return; }
                node gp = search_parent(par.value, n);
                if (gp != null)
                {


                    if (n == head)
                    {
                        n.color = "black";
                        break;
                    }

                    else if (par == gp.right)
                    {
                        if (gp.left == null)
                        {
                            node nil = new node();
                            nil.value = -2938.34738;
                            gp.left = nil;
                            nil.color = "black";
                        }
                        if (par.color == "red" && gp.left.color == "red")
                        {
                            gp.color = "red";
                            par.color = "black";
                            gp.left.color = "black";
                            n = gp;
                            current_x = n.value;
                        }
                        else if (par.color == "red" && gp.left.color == "black")
                        {
                            
                            rotate_R(n, par, gp.left, gp);

                        }
                        else { n = null; }


                    }
                    else if (par == gp.left)
                    {
                        if (gp.right == null)
                        {
                            node nil = new node();
                            nil.value = -2938.34738;
                            gp.right = nil;
                            nil.color = "black";
                        }
                        if (par.color == "red" && gp.right.color == "red")
                        {
                            gp.color = "red";
                            par.color = "black";
                            gp.right.color = "black";
                            n = gp;
                            current_x = n.value;
                        }
                        else if (par.color == "red" && gp.right.color == "black")
                        {
                            rotate_L(n, par, gp.right, gp);
                        }
                        else { n = null; }
                    }

                }
                else { n = null; }
            }
        }
        public void rotate_R(node n, node p, node u, node gp)
        {
            if (p.right == n)
            {
                //right right case

                node ggp = search_parent(gp.value, n);

                //rotate g left
                if (gp == head) { head = p; }
                if (p.left != null) { gp.right = p.left; }
                else { gp.right = null; }
                p.left = gp;
                if (ggp != null) { ggp.right = p; }


                string color;
                color = p.color;
                p.color = gp.color;
                gp.color = color;

                if (ggp != null)
                {
                    n = ggp;
                    current_x = n.value;
                }

            }
            else if (p.left == n)
            {
                //right left case

                //rotate right p
                if (n.right != null) { p.left = n.right; }
                else { p.left = null; }
                n.right = p;
                gp.right = n;

                //right right case
                if (gp == head) { head = n; }
                if (n.left != null) { gp.right = n.left; }
                else { gp.right = null; }
                n.left = gp;
                node ggp = search_parent(gp.value, n);
                if (ggp != null) { ggp.right = n; }

                string color;
                color = n.color;
                n.color = gp.color;
                gp.color = color;

                if (ggp != null)
                {
                    n = ggp;
                    current_x = n.value;
                }
            }
        }
        public void rotate_L(node n, node p, node u, node gp)
        {
            if (p.left == n)
            {
                //left left case
                node ggp = search_parent(gp.value, n);

                if (gp == head) { head = p; }
                if (p.right != null) { gp.left = p.right; }
                else { gp.left = null; }
                p.right = gp;
                if (ggp != null)
                {
                    ggp.left = p;
                }

                string color;
                color = p.color;
                p.color = gp.color;
                gp.color = color;

                if (ggp != null)
                {
                    n = ggp;
                    current_x = n.value;
                }
            }

            else if (p.right == n)
            {
                //left right case

                //rotate p left
                if (n.left != null) { p.right = n.left; }
                else { p.right = null; }
                n.left = p;
                gp.left = n;

                //left left case
                if (gp == head) { head = n; }
                if (n.right != null) { gp.left = n.right; }
                else { gp.left = null; }
                n.right = gp;
                node ggp = search_parent(gp.value, n);
                if (ggp != null) { ggp.left = n; }


                string color;
                color = n.color;
                n.color = gp.color;
                gp.color = color;

                if (ggp != null)
                {
                    n = ggp;
                    current_x = n.value;
                }
            }
        }

        

        public void inorder()
        {
            node n = head;
            inorder_h(n);
        }
        public void inorder_h(node n)
        {

            if (n != null)
            {
                inorder_h(n.left); //nil.value = -2938.34738;
                                   //if (n.value != -2938.34738) { Console.Write(" " + n.value + n.color); }
                Console.Write(" " + n.value + n.color);
                inorder_h(n.right);
            }
        }

        public node search(double x, node n)
        {
            n = head;
            while (n != null)
            {
                if (x > n.value)
                { n = n.right; }

                else if (x < n.value)
                { n = n.left; }
                else if (x == n.value)
                {
                    return n;
                }
            }
            return (null);
        }
        public node search_parent(double x, node n)
        {
            node p, h;
            p = search(x, n);
            h = head;
            while (h != null)
            {
                if (x > h.value)
                {
                    if (h.right == p)
                    {
                        return (h);
                    }
                    h = h.right;
                }
                else if (x < h.value)
                {
                    if (h.left == p)
                    {
                        return (h);
                    }
                    h = h.left;
                }
                else if (x == h.value)
                {
                    return (null);
                }
            }
            return (null);
        }




        public node minimumwaiz(node e)
        {
            if (e.left != null && e.left.value != -2938.34738)
            {
                return minimumwaiz(e.left);
            }
            else
            {
                if (e.value != -2938.34738)
                {
                    double a = e.value;
                } 
                return e;
            }

        }
        public double  Findnum(double v)
        {
            node t = head;
           node   d=Find(t, v);
            return d.value;
        }
        public node Find(node n, double val)
        {
            if (n == null || n.value == val)
            {
                return n;
            }
            if (n.value < val)
                return Find(n.right, val);
            if (n.value > val)
                return Find(n.left, val);
            return n;
        }
        public void DuplicateRef(ref double a, ref double b)
        {
            b = a;
        }
      
        public static void ConvertTreeToList(node root, List<double> result)
        {
            if (root == null)
            {
                return;
            }


            ConvertTreeToList(root.left, result);
            result.Add(root.value);
            ConvertTreeToList(root.right, result);
        }


        
        public void addnill()
        {
            node m = head;
            
            nil.value = -2938.34738;
            nil.color = "black";

            add_nill_node(m);
        }
        public void add_nill_node(node x)
        {
            if (x == nil || x.value == -2938.34738)
            {
                return;
            }
                if (x.left == null && x.left != nil && x.right == null && x.right != nil)
            {
                x.left = nil;
                x.right = nil;
                
            }
            else if (x.left == null && x.left != nil )
            {
                x.left = nil;

            }
            else if (x.right == null && x.right != nil)
            {
                x.right = nil;
            }
            add_nill_node(x.left);
            add_nill_node(x.right);
            return;
        }
        public bool deletionRBT(double val)
        {
            node t = head;
            addnill();
            bool d = deleteRBT(t, val);
            removenil();
            //List<double> a = new List<double>();
            //ConvertTreeToList(t, a);
            //using (StreamWriter sr = new StreamWriter(@"C:\\Users\\HP-15\\Desktop.txt"))
            //{
            //    foreach (double n in a)
            //    {
            //        sr.WriteLine(n);
            //    }
            //}
            return d;

        }
        public bool deleteRBT(node x, double val)
        {
            node f = Find(x, val);
            //addnill();
            if (f == null || f.value == -2938.34738)
                return false;
            //if(x.value == )
            if (x.right != null && x.right.value != -2938.34738)
            {
                if (x.right.value == f.value || x.value == f.value)
                {
                    if (x.right.right != null && x.right.right.value != -2938.34738 && x.right.left != null && x.right.left.value != -2938.34738)
                    {
                        node m = null;
                        if (f.right != null)
                        {
                            m = minimumwaiz(f.right);
                        }
                        //else
                        //{
                        //    m = minimumwaiz(f.left);
                        //} 
                        if (f.color == m.color && m.color == "red")
                        {
                            DuplicateRef(ref m.value, ref f.value);
                            val = m.value;
                            return deleteRBT(f.right, val);

                            //if (f.value == head.value)
                           //{
                            //    return deleteBST(f.right, val);
                            //}
                        }
                        if (f.color == m.color && m.color == "black")
                        {
                            DuplicateRef(ref m.value, ref f.value);
                            val = m.value;
                            return deleteRBT(f.right, val);
                        }
                        if (f.color == "black" && m.color == "red")
                        {
                            DuplicateRef(ref m.value, ref f.value);
                            val = m.value;
                            return deleteRBT(f.right, val);
                        }
                        if (f.color == "red" && m.color == "black")
                        {
                            DuplicateRef(ref m.value, ref f.value);
                            val = m.value;
                            return deleteRBT(f, val);
                        }

                    }
                    if (x.right.right != null )
                    {
                        if (x.right.color == "black")
                        {
                            if (x.right.right.color == "red")
                            {
                                double a = x.right.value;
                                x.right = x.right.right;
                                x.right.color = "black";
                            }
                        }
                        //if (x.right.color == "black")

                            //return true;
                    }
                    if (x.right.left != null )
                    {
                        if (x.right.color == "black")
                        {
                            if (x.right.left.value == f.value && x.right.left.color == "red")
                            {
                                double a = x.right.value;
                                x.right = x.right.left;
                                x.right.color = "black";
                                return true;
                            }
                        }
                        if (x.right.color == "black" || x.right.color == "red")
                        {
                            if (x.right.left.value == f.value && x.right.left.color == "black")
                            {
                                x.right.left.color = "DB";
                                Casematch_R(x.right);
                                addnill();
                            }
                        }
                    }
                    if(x.right.value == f.value)
                    {
                        if (x.right.color == "red")
                        {
                            double a = x.right.value;
                            x.right = null;
                            return true;
                        }
                        if (x.color == "black" || x.color == "red")
                        {
                            if (x.right.value == f.value && x.right.color == "black")
                            {
                                x.right.color = "DB";
                                x.right.value = 0;
                                Casematch_R(x);
                                addnill();
                            }
                        }
                        //if (x.color == "red")
                        //{
                        //    if (x.right.value == f.value && x.right.color == "black")
                        //    {
                        //        x.
                        //    }
                        //}


                    }
                }
            }
            if (x.left != null && x.left.value != -2938.34738)
            {
                if (x.left.value == f.value || x.value == f.value)
                {
                    if (x.left.right != null && x.left.right.value != -2938.34738 && x.left.left != null && x.left.left.value != -2938.34738)
                    {
                        node m;
                        if (f.right != null)
                        {
                            m = minimumwaiz(f.right);
                        }
                        else
                        {
                            m = minimumwaiz(f.left);
                        }
                        if (f.color == m.color && m.color == "red")
                        {
                            DuplicateRef(ref m.value, ref f.value);
                            val = m.value;
                            if (f.value == head.value)
                            {
                                return deleteRBT(f.right, val);
                            }
                        }
                        if (f.color == "black" && m.color == "red")
                        {
                            DuplicateRef(ref m.value, ref f.value);
                            val = m.value;
                            return deleteRBT(f, val);
                        }
                        if (f.color == "red" && m.color == "black")
                        {
                            DuplicateRef(ref m.value, ref f.value);
                            val = m.value;
                            return deleteRBT(f, val);
                        }
                    }
                    if (x.left.right != null && x.left.right.value != -2938.34738)
                    {
                        if (x.left.color == "black")
                        {
                            if (x.left.right.color == "red")
                            {
                                double a = x.left.value;
                                x.left = x.left.right;
                                x.left.color = "black";
                                return true;
                            }
                        }

                    }
                    if (x.left.left != null && x.left.left.value != -2938.34738)
                    {
                        if (x.left.color == "black")
                        {
                            if (x.left.left.color == "red")
                            {
                                double a = x.left.value;
                                x.left = x.left.left;
                                return true;
                            }
                        }
                    }
                    if(x.left.value == f.value)
                    {
                        if (x.left.color == "red")
                        {
                            double a = x.left.value;
                            x.left = null;
                            return true;
                        }
                        if (x.color == "black" || x.color == "red")
                        {
                            if (x.left.value == f.value && x.left.color == "black")
                            {
                                x.left.color = "DB";

                                Casematch_L(x);

                            }
                        }

                    }
                }

            }
            if (x.value < val)
            {
                return deleteRBT(x.right, val);
            }
            if (x.value > val)
            {
                return deleteRBT(x.left, val);
            }
            return false;
        }
        public node nil = new node();
        
        public void Casematch_L(node x)
        {
            if (x == head) //CASE I 
            {
                x.color = "black";
            }
            if (x.color == "black")
            {
                //CASE II
                if (x.right.color == "red" && x.left.color == "DB")
                {
                    if (x.right.right.color == "black"  && x.right.left.color == "black" )
                    {
                        node gp = search_parent(x.value, x);
                        node S = x.right;
                        x.right = x.right.left;
                        S.left = x;
                        S.color = "black";
                        x.color = "red";
                        if (gp != null)
                        {
                            if (gp.right.value == x.value)
                            {
                                gp.right = S;
                            }
                            else
                            {
                                gp.left = S;
                            }
                        }

                        //addnill();
                        Casematch_L(x);
                        
                        return;
                    }
                }
                //CASE III
                if (x.right.color == "black" && x.left.color == "DB")
                {
                    if (x.right.right.color == "black" && x.right.left.color == "black" )
                    {
                        x.color = "DB";
                        x.right.color = "red";
                        x.left = nil;
                        
                        //addnill();
                        node gp = search_parent(x.value, x);
                        if (gp != null)
                        {
                            if (gp.right.color == "DB")
                            {
                                Casematch_R(gp);
                            }
                            else
                            {
                                Casematch_L(gp);
                            }
                        }
                        else
                        {

                            Casematch_L(x);
                        }
                            
                        
                        return;
                    }
                    
                }
            }
            if (x.color == "red") //CASE IV
            {
                if (x.right.color == "black" && x.left.color == "DB")
                {
                    if (x.right.right.color == "black"  && x.right.left.color == "black" )
                    {
                        x.color = "black";
                        x.left = nil;
                        x.right.color = "red";
                        return;
                    }
                }
            }
            if (x.color == "red" || x.color == "black")
            {
                if (x.left.color == "DB" && x.right.color == "black")
                {
                    //CASE V
                    if (x.right.left.color == "red" && x.right.right.color == "black" || x.right.right == null)
                    {
                        //if (x == head)
                        //{
                            node S = x.right;
                            node xleft = S.left.right;
                            x.right = S.left;
                            x.right.right = S;
                            S.left = xleft;
                            //x.color = "P";
                            x.right.color = "black";
                            S.color = "red";
                        // }
                        //else
                        //{

                        //}
                        
                        //addnill();
                        Casematch_L(x);
                        return;
                    }
                    //CASE VI
                    if (x.right.right.color == "red" && x.right.left.color == "black" || x.right.left.color == "red" || x.right.left == null)
                    {
                        node gp = search_parent(x.value, x);
                        
                        node S = x.right;
                        x.right = x.right.left;
                        S.left = x;
                        x.color = "black";
                        x.left = null;
                        S.color = "red";
                        //x.right.color = "P";
                        S.right.color = "black";
                        if (gp != null)
                        {
                            if (gp.right.value == x.value)
                            {
                                gp.right = S;
                            }
                            else
                            {
                                gp.left = S;
                            }
                        }
                        else
                        {
                            S.color = "black";
                            head = S;
                        }
                        return;
                        
                    }
                }
            }
        }
        public void removenil() //Remove nill nodes
        {
            node t = head;
            remove_nill(t);
        }
        public void remove_nill(node x)
        {
            if (x == null)
            {
                return;
            }
            if (x.right != null)
            {
                if (x.right.value == -2938.34738 )
                {
                    x.right = null;
                }
            }
            if (x.left != null)
            {
                if (x.left.value == -2938.34738 )
                {
                    x.left = null;
                }
            }
            
            remove_nill(x.left);
            //Console.WriteLine(x.value);
            remove_nill(x.right);
            return;
        }
        public void Casematch_R(node x)
        {
            if (x == head) //CASE I 
            {
                x.color = "black";
            }
            if (x.color == "black") 
            {
                if (x.right.color == "DB" && x.left.color == "red") //CASE II
                {
                    if (x.left.left.color == "black"  && x.left.right.color == "black" )
                    {
                        node gp = search_parent(x.value, x);
                        node S = x.left;
                        x.left = x.left.right;
                        S.right = x;
                        S.color = "black";
                        x.color = "red";
                        if (gp != null)
                        {
                            if (gp.left.value == x.value)
                            {
                                gp.left = S;
                            }
                            else
                            {
                                gp.right = S;
                            }
                        }
                        addnill();
                        Casematch_R(x);
                        return;

                    }
                }
                if (x.right.color == "DB" && x.left.color == "black") //CASE III
                {
                    if (x.left.left.color == "black"  && x.left.right.color == "black" )
                    {
                        x.color = "DB";
                        x.left.color = "red";
                        x.right.color = "black";
                        //addnill();
                        node gp = search_parent(x.value, x);
                        if (gp != null)
                        {
                            if (gp.right.color == "DB")
                            {
                                Casematch_R(gp);
                            }
                            else
                            {
                                Casematch_L(gp);
                            }
                        }
                        else
                        {

                            Casematch_R(x);
                        }
                        return;
                    }

                }
            }
            if (x.color == "red") //CASE IV
            {
                if (x.right.color == "DB" && x.left.color == "black")
                {
                    if (x.left.right.color == "black" || x.left.right == nil  && x.left.left.color == "black" || x.left.left == nil)
                    {
                        x.color = "black";
                        x.right.color = "black";
                        x.left.color = "red";
                        x.right = null;
                        return;
                    }
                    
                }
            }
            if (x.color == "red" || x.color == "black")
            {
                if (x.left.color == "black" && x.right.color == "DB") //CASE V
                {
                    if (x.left.left.color == "red" && x.left.right.color == "black" )
                    {
                        //if (x == head)
                        //{
                            node S = x.left;
                            node xright = S.right.left;
                            x.left = S.right;
                            x.left.left = S;
                            S.right = xright;
                            //x.color = "P";
                            x.left.color = "black";
                            S.color = "red";
                           
                            S.left.color = "black";
                        //addnill();
                        //}
                        //else
                        //{
                        //    node S = x.left;
                        //    node xright = S.right.left;
                        //    x.left = S.right;
                        //    x.left.left = S;
                        //    S.right = xright;
                        //    //x.color = "P";
                        //    x.left.color = "black";
                        //    S.color = "red";
                        //    S.left.color = "black";
                        //    //addnill();
                        //}

                        addnill();
                        Casematch_R(x); 
                        return;
                    }
                    //CASE VI
                    if (x.left.right.color == "red" &&x.left.right !=nil && x.left.left != nil && x.left.left.color == "black" || x.left.left.color == "red" || x.left.left == null)
                    {
                        node gp = search_parent(x.value, x);

                        node S = x.left;
                        x.left = x.left.right;
                        S.right = x;
                        x.color = "black";
                        x.right = null;
                        S.color = "black";
                        //x.right.color = "P";
                        S.right.color = "black";
                        if (gp != null)
                        {
                            if (gp.left.value == x.value)
                            {
                                gp.left = S;
                            }
                            else
                            {
                                gp.right = S;
                            }
                        }
                        else
                        {
                            S.color = "black";
                            head = S;
                        }
                        addnill();
                        return;
                    }
                }
            }
        }
    }
}

