/*
 * Copyright 2006 Senselogic
 *
 *  Licensed under the Apache License, Version 2.0 (the "License");
 *  you may not use this file except in compliance with the License.
 *  You may obtain a copy of the License at
 *
 *     http://www.apache.org/licenses/LICENSE-2.0
 *
 *  Unless required by applicable law or agreed to in writing, software
 *  distributed under the License is distributed on an "AS IS" BASIS,
 *  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 *  See the License for the specific language governing permissions and
 *  limitations under the License.
 */

package senselogic.excelbundle;

import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import org.dom4j.DocumentException;

/**
 * Command line interface.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class Main
{
	// Static --------------------------------------------------------
	private static File root;
	private static File outputFile;
	private static File inputFile;
	private static File sheetMapFile;
	private static Map<String, String> sheetMap;
	private static String strRefLang = "en";
	private static List<String> strLanguages = new ArrayList<String>();
	private static List<String> untransList = new ArrayList<String>();
	private static Map<String, LanguagePack> languages = 
		new LinkedHashMap<String, LanguagePack>();
	private static boolean pretend = false;
	
	private static LanguageTreeIO tree;
	
	// Public --------------------------------------------------------
	public static void main(String[] args)
	{
		if(args.length == 0)
		{
			printUsage();
			System.exit(1);
		}
		parseArgs(args);
		
		if(inputFile != null)
			doImport();
		else if(outputFile != null)
			doExport();
	}
	
	// Private -------------------------------------------------------
	private static void printUsage()
	{
		System.out.println(" Syntax:\n" + 
				"   java -jar excelbundle.jar -r <root> (-export|-import) <excel file> [-p]\n" + 
				"   -l <languages> [-u <languages>] [-ref <reference language>] [-m <sheet map>]\n" + 
				"  \n" + 
				" Explanation:\n" + 
				"   -r      - The root of the source tree. In export mode, all paths in the \n" + 
				"             resulting Excel spreadsheet will be relative to this path. In\n" + 
				"             import mode, all paths in the source Excel spreadsheet will be\n" + 
				"             assumed to be relative to this path.\n" + 
				"            \n" + 
				"   -export - Exports resource bundles from the specified root to <excel file>.\n" + 
				"  \n" + 
				"   -import - Import resource bundles from <excel file> to the specified root.\n" + 
				"   \n" + 
				"   -p      - Only pretend doing an import. When this option is specified in\n" + 
				"             import mode, nothing is actually imported. excelbundle only\n" + 
				"             displays what would be done if this option was not specified.\n" + 
				"  \n" + 
				"   -l      - A comma separated list of language codes. In export mode, this\n" + 
				"             specifies what languages should be included in the resulting Excel\n" + 
				"             spreadsheet. In import mode, this specified what langauges are to\n" + 
				"             be merged back into the source tree. To specify the \n" + 
				"             \"default language\", use \"default\".\n" + 
				"  \n" + 
				"   -u      - A comma separated list of language codes. When this is specified,\n" + 
				"             only keys which are untranslated in at least one of the specified\n" + 
				"             languages are included into the resulting Excel spreadsheet.\n" + 
				"            \n" + 
				"   -ref    - See description in README for detailed explanation. Defaults to \"en\".\n" + 
				"            \n" + 
				"   -m      - Specifies a sheet map file to use. A sheet map file can be used to\n" + 
				"             categorize different bundles into different sheet of the workbook\n" + 
				"             in the resulting Excel spreadsheet. An example is provided in the\n" + 
				"             sheetmap.xml.sample file included in the root of the distribution.");
	}
	
	private static void parseArgs(String[] argsArray)
	{
		//Add stuff to an ArrayList for easier handling
		List<String> args = new ArrayList<String>();
		for(String arg : argsArray)
			args.add(arg);
		
		Iterator<String> it = args.iterator();
		while(it.hasNext())
		{
			String arg = it.next();
			
			if(arg.equals("-r"))
			{
				if(!it.hasNext())
				{
					System.out.println("Root folder is missing!");
					printUsage();
					System.exit(1);
				}
				root = new File(it.next());
			}
			else if(arg.equals("-export"))
			{
				if(!it.hasNext())
				{
					System.out.println("Output file is missing!");
					printUsage();
					System.exit(1);
				}
				outputFile = new File(it.next());
			}
			else if(arg.equals("-import"))
			{
				if(!it.hasNext())
				{
					System.out.println("Input file is missing!");
					printUsage();
					System.exit(1);
				}
				inputFile = new File(it.next());
			}
			else if(arg.equals("-l"))
			{
				if(!it.hasNext())
				{
					System.out.println(
							"You must specify what languages to include!");
					printUsage();
					System.exit(1);
				}
				String[] langs = it.next().split(",");
				for(String lang : langs)
					strLanguages.add(lang);
			}
			else if(arg.equals("-ref"))
			{
				if(!it.hasNext())
				{
					System.out.println("" +
							"You must specify a reference langauge.");
					printUsage();
					System.exit(1);
				}
				strRefLang = it.next();
			}
			else if(arg.equals("-m"))
			{
				if(!it.hasNext())
				{
					System.out.println("You must specify a sheet map file.");
					printUsage();
					System.exit(1);
				}
				sheetMapFile = new File(it.next());
			}
			else if(arg.equals("-u"))
			{
				if(!it.hasNext())
				{
					System.out.println(
							"You must specify untranslated subset languages!");
					printUsage();
					System.exit(1);
				}
				String[] langs = it.next().split(",");
				for(String lang : langs)
					untransList.add(lang);
			}
			else if(arg.equals("-p"))
				pretend = true;
		}
		
		//Let's see if everything was specified
		if(root == null)
		{
			System.out.println(
					"You need to specify the root folder of the source tree.");
			printUsage();
			System.exit(1);
		}
		if((inputFile == null) && (outputFile == null))
		{
			System.out.println(
					"You need to specify either -export or -import.");
			printUsage();
			System.exit(1);
		}
		if(strLanguages.isEmpty())
		{
			System.out.println("You need to specify at least one language.");
			printUsage();
			System.exit(1);
		}
		if((inputFile != null) && strLanguages.contains(strRefLang))
		{
			System.out.println(
					"You cannot import the language you've specified as" +
					"reference.");
			printUsage();
			System.exit(1);
		}
	}
	
	/**
	 * Does the import-languages-into-tree stuff.
	 */
	private static void doImport()
	{
		ImportAction importAction = new ImportAction();
		importAction.setInputFile(inputFile);
		importAction.setLanguages(strLanguages);
		importAction.setRoot(root);
		importAction.setRefLang(strRefLang);
		importAction.setPretend(pretend);
		try
		{
			importAction.doImport(System.out);
		}
		catch(IOException e)
		{
			System.err.println("Error while importing:");
			e.printStackTrace();
			System.exit(2);
		}
	}
	
	/**
	 * Exports languages to excel file.
	 */
	private static void doExport()
	{
		try
		{
			if(sheetMapFile != null)
			{
				sheetMap = ExportAction.createSheetMapping(sheetMapFile);
			}
		}
		catch(DocumentException e)
		{
			System.out.println("Unable to parse sheet mapping file:");
			e.printStackTrace();
			System.exit(2);
		}
		
		ExportAction exportAction = new ExportAction();
		exportAction.setLanguages(strLanguages);
		exportAction.setUntransLangs(untransList);
		exportAction.setRoot(root);
		exportAction.setSheetMap(sheetMap);
		exportAction.setOutputFile(outputFile);
		exportAction.setRefLang(strRefLang);
		try
		{
			exportAction.export(System.out);
		}
		catch(IOException e)
		{
			System.err.println("Error while exporting:");
			e.printStackTrace();
			System.exit(2);
		}
	}
}