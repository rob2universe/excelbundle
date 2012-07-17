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

/**
 * Class for merging LanguagePacks into a LanguageTreeIO.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class Merger
{
    // Attributes ----------------------------------------------------
	private LanguageTreeIO tree;
    private LanguagePack refLang;
    
    private int filesCreated;
	private int filesDeleted;
	private int filesChanged;
	
	private int keysAdded;
	private int keysDeleted;
	private int keysChanged;

	
    // Constructors --------------------------------------------------
    /**
     * Constructs a new LanguageTreeIO.
     * 
     * @param tree     the LanguageTreeIO to merge to
     * @param refLang  the language to use as a reference for what keys and 
     *                 files are to be added or removed
     */
    public Merger(LanguageTreeIO tree, LanguagePack refLang)
	{
		this.tree = tree;
		this.refLang = refLang;
	}
    
    // Public --------------------------------------------------------
    /**
     * Merges the specified langauge into the source tree.
     */
    public void merge(LanguagePack pack, boolean pretend) throws IOException
    {
    	for(LanguageFile langFile : pack.getLanguageFiles())
    		merge(langFile, pretend);
    }

    /**
     * Merges the specified LanguageFile into the source tree. If the file on 
     * disk does not differ from the specified LanguageFile, nothing is done at
     * all.
     */
    public void merge(LanguageFile langFile, boolean pretend) 
    	throws IOException
    {
    	LanguageFile refLangFile = refLang.getLanguageFile(langFile.getPath());
    	//File does not exist, remove the file if it exists
    	if(refLangFile == null) 
    	{
    		File file = new File(tree.getRoot(), langFile.getFilename());
    		if(file.exists())
    		{
    			fileDeleted(langFile.getFilename());
    			if(!pretend)
    				file.delete();
    		}
    		return;
    	}
    	
    	LanguageFile existingFile = tree.loadLanguageFile(
    			langFile.getPath(), langFile.getLanguage());
    	LanguageFile finalFile = langFile;
    	if(existingFile != null)
    		finalFile = existingFile.merge(langFile, true);

    	finalFile = finalFile.commonKeySubset(refLangFile);
    	
    	if(existingFile == null)
    	{
    		fileCreated(langFile.getFilename());
    		if(!pretend)
    			tree.save(langFile);
    	}
    	else
    	{
    		if(existingFile.differs(finalFile))
    		{
    			fileChanged(finalFile.getFilename());
    			if(!pretend)
    				tree.save(finalFile);
    		}
    	}
    	
    	//And some stats.
    	keyStatistics(existingFile, finalFile);
    }
 	
	public int getFilesCreated()
	{
		return filesCreated;
	}
	
	public int getFilesChanged()
	{
		return filesChanged;
	}
	
	public int getFilesDeleted()
	{
		return filesDeleted;
	}
	
	public int getKeysAdded()
	{
		return keysAdded;
	}
	public int getKeysChanged()
	{
		return keysChanged;
	}
	
	public int getKeysDeleted()
	{
		return keysDeleted;
	}

	public void clearStatistics()
	{
		filesCreated = 0;
		filesDeleted = 0;
		filesChanged = 0;
		
		keysAdded = 0;
		keysDeleted = 0;
		keysChanged = 0;
	}
	
	public LanguagePack getReferenceLanguage()
	{
		return refLang;
	}

	// Private -------------------------------------------------------
	/*
	 * You could use listeners for these methods instead, but it feels overkill
	 * for now.
	 */
	private void fileCreated(String filename)
	{
		System.out.println("* Creating file " + filename);
		filesCreated++;
	}

	private void fileChanged(String filename)
	{
		System.out.println("* Changing file " + filename);
		filesChanged++;
	}

	private void fileDeleted(String filename)
	{
		System.out.println("* Deleting file " + filename);
		filesDeleted++;
	}

	private void keyAdded(String filename, String key)
	{
		System.out.println("* - Adding key " + key + " in file " + filename);
		keysAdded++;
	}

	private void keyChanged(String filename, String key)
	{
		System.out.println("* - Changing key " + key + " in file " + filename);
		keysChanged++;
	}

	private void keyDeleted(String filename, String key)
	{
		System.out.println("* - Deleting key " + key + " in file " + filename);
		keysDeleted++;
	}
	
    private void keyStatistics(LanguageFile existingFile, LanguageFile newFile)
    {
    	if(existingFile == null)
    	{
    		for(LanguageFile.KeyValuePair pair : newFile.getPairs())
    			keyAdded(newFile.getFilename(), pair.getKey());
    		return;
    	}
    		
    	for(LanguageFile.KeyValuePair pair : existingFile.getPairs())
    	{
    		String key = pair.getKey();
    		if(newFile.getValue(key) == null)
    			keyDeleted(existingFile.getFilename(), key);
    		else if(!newFile.getValue(key).equals(pair.getValue()))
    			keyChanged(existingFile.getFilename(), key);
    	}
    	for(LanguageFile.KeyValuePair pair : newFile.getPairs())
    	{
    		if(existingFile.getValue(pair.getKey()) == null)
    			keyAdded(existingFile.getFilename(), pair.getKey());
    	}
    }
}