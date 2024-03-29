using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

class Program
{
    static void Main()
    {
        // Chemin du fichier contenant le dictionnaire de mots
        string path = "chemin_vers_le_fichier_dictionnaire.txt";
        
        // Mots fournis par l'utilisateur
        List<string> motsUtilisateur = new List<string> { "omee", "teac", "tiodi", "xsc" };

        // Charger les mots du dictionnaire depuis le fichier
        List<string> motsDictionnaire = LoadDictionary(path);

        // Résoudre le problème pour chaque mot fourni par l'utilisateur
        foreach (string motUtilisateur in motsUtilisateur)
        {
            string motTrouve = FindWord(motUtilisateur, motsDictionnaire);
            if (motTrouve != null)
            {
                Console.WriteLine($"{motUtilisateur} correspond à {motTrouve}");
            }
            else
            {
                Console.WriteLine($"Aucune correspondance trouvée pour {motUtilisateur}");
            }
        }

        Console.ReadLine();
    }

    static List<string> LoadDictionary(string path)
    {
        List<string> motsDictionnaire = new List<string>();

        try
        {
            motsDictionnaire = File.ReadAllLines(path).ToList();
        }
        catch (FileNotFoundException)
        {
            Console.WriteLine("Le fichier du dictionnaire est introuvable.");
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Une erreur s'est produite lors du chargement du dictionnaire : {ex.Message}");
        }

        return motsDictionnaire;
    }

    static string FindWord(string motUtilisateur, List<string> motsDictionnaire)
    {
        foreach (string motDictionnaire in motsDictionnaire)
        {
            if (IsAnagram(motUtilisateur, motDictionnaire))
            {
                return motDictionnaire;
            }
        }

        return null;
    }

    static bool IsAnagram(string mot1, string mot2)
    {
        if (mot1.Length != mot2.Length)
        {
            return false;
        }

        char[] mot1Chars = mot1.ToCharArray();
        char[] mot2Chars = mot2.ToCharArray();

        Array.Sort(mot1Chars);
        Array.Sort(mot2Chars);

        for (int i = 0; i < mot1Chars.Length; i++)
        {
            if (mot1Chars[i] != mot2Chars[i])
            {
                return false;
            }
        }

        return true;
    }
}
