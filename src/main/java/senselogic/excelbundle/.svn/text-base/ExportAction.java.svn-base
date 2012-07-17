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
import java.util.ArrayList;
import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;

import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

/**
 * Easy to use interface class for exporting Excel files.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class ExportAction
{
	// Static --------------------------------------------------------
	public static Map<String, String> createSheetMapping(File sheetMapFile)
		throws DocumentException
	{
		Map<String, String> sheetMap = new HashMap<String, String>();
		SAXReader reader = new SAXReader();
		Document doc = reader.read(sheetMapFile);
		Element root = doc.getRootElement();
		Iterator it = root.elements("mapping").iterator();
		while(it.hasNext())
		{
			Element mapping = (Element)it.next();
			sheetMap.put(mapping.getText(), mapping.attributeValue("name"));
		}
		
		return sheetMap;
	}
	
    // Attributes ----------------------------------------------------
	private List<String> strLanguages;
	private List<String> untransList;
	private Map<String, String> sheetMap;
	private File root;
	private File outputFile;
	private String strRefLang;
	
    // Public --------------------------------------------------------
	public void setLanguages(List<String> strLanguages)
	{
		this.strLanguages = strLanguages;
	}
	
	public void setUntransLangs(List<String> untransList)
	{
		this.untransList = untransList;
	}
	
	public void setSheetMap(Map<String, String> sheetMap)
	{
		this.sheetMap = sheetMap;
	}
	
	public void setRoot(File root)
	{
		this.root = root;
	}
	
	public void setOutputFile(File outputFile)
	{
		this.outputFile = outputFile;
	}
	
	public void setRefLang(String strRefLang)
	{
		this.strRefLang = strRefLang;
	}
	
	/**
	 * Exports with the current settings.
	 * 
	 * @param out  what stream to print information to about the export, or 
	 *             null if silenced
	 */
	public void export(PrintStream out) throws IOException
	{
		LanguageTreeIO tree = null;
		try
		{
			tree = new LanguageTreeIO(root);
		}
		catch(IOException e)
		{
			IOException e2 = new IOException(
					"Error while source tree for bundles");
			e2.initCause(e);
			throw e2;
		}
		
		//Load languages
		Map<String, LanguagePack> languages =
			new LinkedHashMap<String, LanguagePack>();
		try
		{
			for(String strLang : strLanguages)
				languages.put(strLang, tree.loadLanguage(strLang));
		}
		catch(IOException e)
		{
			IOException e2 = new IOException(
					"Error while loading langauges from source tree");
			e2.initCause(e);
			throw e2;
		}
		
		//Let's load the reflang from the list of languages first
		LanguagePack refLang = languages.get(strRefLang);
		//Otherwise, load it from the tree
		if(refLang == null)
		{
			try
			{
				refLang = tree.loadLanguage(strRefLang);
			}
			catch(IOException e)
			{
				IOException e2 = new IOException(
						"Error while loading reference language");
				e2.initCause(e);
				throw e2;
			}
		}
		
		//If we want untranslated only...
		if(!untransList.isEmpty())
		{
			List<LanguagePack> untrans = new ArrayList<LanguagePack>();
			for(String strUntrans : untransList)
				untrans.add(languages.get(strUntrans));
			refLang = refLang.missingKeySubset(untrans);
		}
		
		//Export...
		ExcelExporter exporter = new ExcelExporter();
		exporter.setReferenceLanguage(refLang);
		if(sheetMap != null)
			exporter.setSheetMap(sheetMap);
		
		for(LanguagePack language : languages.values())
			exporter.addLanguagePack(language);
		
		try
		{
			//If we have the untranslated subset of _one_ language, we don't 
			//need to redmark all of the missing cells as one whole column will
			//be missing anyway
			if(untransList.size() == 1)
				exporter.write(outputFile, false);
			else
				exporter.write(outputFile, true);
			
			//Print some info
			if(out != null)
			{
				out.println(refLang.keyCount() + " keys from " + 
						refLang.getLanguageFiles().size() + " files exported");
			}
		}
		catch(IOException e)
		{
			IOException e2 = new IOException(
					"Unable to write Excel file");
			e2.initCause(e);
			throw e2;
		}
	}
}