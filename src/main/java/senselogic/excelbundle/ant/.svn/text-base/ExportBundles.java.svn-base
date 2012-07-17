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

package senselogic.excelbundle.ant;

import java.io.File;
import java.io.IOException;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.apache.tools.ant.BuildException;
import org.apache.tools.ant.Task;
import org.dom4j.DocumentException;

import senselogic.excelbundle.ExportAction;

/**
 * Ant task for exporting a resources to an Excel file.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class ExportBundles extends Task
{
    // Attributes ----------------------------------------------------
    private File root;
    private File destfile;
    private File sheetMapFile;
    private List<String> strLangList = new ArrayList<String>();
    private String refLang = "en";
    private List<String> strUntransList = new ArrayList<String>();
    private Map<String, String> sheetMap = new HashMap<String, String>();
    
    // Public --------------------------------------------------------
    public void setSheetMap(File sheetMapFile)
    {
    	this.sheetMapFile = sheetMapFile;
    }
    
	public void setDestfile(File destfile)
	{
		this.destfile = destfile;
	}
	
	public void setLanguages(String strLangs)
	{
		strLangList.clear();
		String[] split = strLangs.split(",");
		for(String lang : split)
			strLangList.add(lang);
	}
	
	public void setRefLang(String refLang)
	{
		this.refLang = refLang;
	}
	
	public void setRoot(File root)
	{
		this.root = root;
	}
	
	public void setUntrans(String strUntrans)
	{
		strUntransList.clear();
		String[] split = strUntrans.split(",");
		for(String lang : split)
			strUntransList.add(lang);
	}
	
	public void addConfiguredSheetmapping(SheetMapping mapping)
	{
		sheetMap.put(mapping.getPrefix(), mapping.getName());
	}
	
	public void execute() throws BuildException
	{
		if(root == null)
			throw new BuildException("No root specified.");
		if(destfile == null)
			throw new BuildException("No destination file specified.");
		if(strLangList.isEmpty())
			throw new BuildException("No languages are specified.");
		
		try
		{
			if(sheetMapFile != null)
				sheetMap = ExportAction.createSheetMapping(sheetMapFile);
		}
		catch(DocumentException e)
		{
			throw new BuildException("Unable to parse sheet map file", e);
		}
		
		try
		{
			ExportAction exportAction = new ExportAction();
			exportAction.setLanguages(strLangList);
			exportAction.setOutputFile(destfile);
			exportAction.setRefLang(refLang);
			exportAction.setRoot(root);
			exportAction.setSheetMap(sheetMap);
			exportAction.setUntransLangs(strUntransList);
			exportAction.export(System.out);
		}
		catch(IOException e)
		{
			throw new BuildException("Unable to export bundles", e);
		}
	}
}