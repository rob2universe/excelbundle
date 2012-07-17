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
import java.io.PrintStream;
import java.util.List;

/**
 * Easy to use interface class for importing.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class ImportAction
{
    // Attributes ----------------------------------------------------
	private List<String> strLanguages;
	private String strRefLang;
	private File root;
	private File inputFile;
	private boolean pretend = false;
	
    // Public --------------------------------------------------------
	public void setLanguages(List<String> strLanguages)
	{
		this.strLanguages = strLanguages;
	}
	
	public void setRefLang(String strRefLang)
	{
		this.strRefLang = strRefLang;
	}
	
	public void setRoot(File root)
	{
		this.root = root;
	}
	
	public void setInputFile(File inputFile)
	{
		this.inputFile = inputFile;
	}
	
	public void setPretend(boolean pretend)
	{
		this.pretend = pretend;
	}
	
	/**
	 * Import with the current settings.
	 * 
	 * @param out  what stream to print information and statistics about the
	 *             import to or null if silenced
	 */
	public void doImport(PrintStream out) throws IOException
	{
		LanguageTreeIO tree = null;
		try
		{
			tree = new LanguageTreeIO(root);
		}
		catch(IOException e)
		{
			IOException e2 = new IOException(
					"Error while scanning source tree for bundles");
			e2.initCause(e);
			throw e2;
		}
		
		LanguagePack refLang = null;
		try
		{
			refLang = tree.loadLanguage(strRefLang);
		}
		catch(IOException e)
		{
			IOException e2 = new IOException(
					"Error while loading reference language from source tree");
			e2.initCause(e);
			throw e2;
		}
		
		ExcelImporter importer = null;
		try
		{
			importer = new ExcelImporter(inputFile);
		}
		catch(IOException e)
		{
			IOException e2 = new IOException(
					"Error while loading excel file");
			e2.initCause(e);
			throw e2;
		}
		
		Merger merger = new Merger(tree, refLang);
		try
		{
			for(String lang : strLanguages)
			{
				//Let's find out how many keys that were translated before
				int before = tree.loadLanguage(lang)
						.commonKeySubset(refLang).keyCount();
				
				merger.clearStatistics();
				LanguagePack excelPack = importer.loadLanguage(lang);
				if(excelPack == null)
				{
					System.out.println(
							lang + " not in excel file, ignoring..,");
					continue;
				}
				
				//Merge
				merger.merge(excelPack, pretend);
				
				//And print stats
				
				if(out != null)
				{
					out.println();
					out.println("= Statistics (" + lang + ") ===============");
					out.println();
					out.println("Files created: " + merger.getFilesCreated());
					out.println("Files deleted: " + merger.getFilesDeleted());
					out.println("Files changed: " + merger.getFilesChanged());
					out.println("Keys added: " + merger.getKeysAdded());
					out.println("Keys deleted: " + merger.getKeysDeleted());
					out.println("Keys changed: " + merger.getKeysChanged());
					out.println();
					
					int total = refLang.keyCount();
					int totalInFile = excelPack.keyCount();
					out.println("Total number of translated keys in file: " +
							totalInFile);
					
					int percentage =
						(int)(((float)before / (float)total) * 100f);
					out.println("Translated keys before merge: " + 
							before + " (" + percentage + "%)");
					
					int after = before + merger.getKeysAdded();
					percentage = (int)(((float)after / (float)total) * 100f);
					out.println("Translated keys after merge: " + 
							after + " (" + percentage + "%)");
				}
			}
		}
		catch(IOException e)
		{
			IOException e2 = new IOException(
					"Error while merging into language tree");
			e2.initCause(e);
			throw e2;
		}
	}
}

