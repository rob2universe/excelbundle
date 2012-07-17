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
import java.util.List;

import org.apache.tools.ant.BuildException;
import org.apache.tools.ant.DirectoryScanner;
import org.apache.tools.ant.Task;
import org.apache.tools.ant.types.FileSet;

import senselogic.excelbundle.ImportAction;

/**
 * Ant task for importing bundles.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class ImportBundles extends Task
{
	// Attributes ----------------------------------------------------
    private File root;
    private File srcfile;
    private List<FileSet> fileSets = new ArrayList<FileSet>();
    private List<String> strLangList = new ArrayList<String>();
    private String refLang = "en";
    private boolean pretend = false;
    
    // Public --------------------------------------------------------
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
	
	public void addFileSet(FileSet fileSet)
	{
		fileSets.add(fileSet);
	}
	
	public void setPretend(boolean pretend)
	{
		this.pretend = pretend;
	}
	
	public void setSrcfile(File srcfile)
	{
		this.srcfile = srcfile;
	}
	
	public void execute() throws BuildException
	{
		if(root == null)
			throw new BuildException("No root specified.");
		if(strLangList.isEmpty())
			throw new BuildException("No languages are specified.");
		
		try
		{
			if(fileSets.isEmpty())
			{
				doImport(srcfile);
				return;
			}
			
			for(FileSet fileSet : fileSets)
			{
				DirectoryScanner scanner =
					fileSet.getDirectoryScanner(getProject());
				String[] files = scanner.getIncludedFiles();
				for(String file : files)
					doImport(new File(fileSet.getDir(getProject()), file));
			}
		}
		catch(Exception e)
		{
			e.printStackTrace();
			throw new BuildException(e);
		}
	}
    
    // Private -------------------------------------------------------
	private void doImport(File excelFile) throws IOException
	{
		ImportAction importAction = new ImportAction();
		importAction.setInputFile(excelFile);
		importAction.setLanguages(strLangList);
		importAction.setRefLang(refLang);
		importAction.setRoot(root);
		importAction.setPretend(pretend);
		importAction.doImport(System.out);
	}
}