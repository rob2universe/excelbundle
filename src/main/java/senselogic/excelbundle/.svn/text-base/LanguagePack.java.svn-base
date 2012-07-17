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

import java.util.ArrayList;
import java.util.Collection;
import java.util.LinkedHashMap;
import java.util.Map;

/**
 * Data class for keeping language files together in a handy package.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class LanguagePack
{
    // Attributes ----------------------------------------------------
    private String language;
    private Map<String, LanguageFile> langFiles =
    	new LinkedHashMap<String, LanguageFile>();
    
    // Constructors --------------------------------------------------
    /**
     * Constructs a new LanguagePack.
     * 
     * @param language  the language of the pack
     */
	public LanguagePack(String language)
	{
		this.language = language;
	}
	
    // Public --------------------------------------------------------
	public String getLanguage()
	{
		return language;
	}
	
	/**
	 * Adds a LanguageFile.
	 */
	public void addLanguageFile(LanguageFile file)
	{
		langFiles.put(file.getPath(), file);
	}
	
	/**
	 * Removes the LanguageFile with the specified path.
	 */
	public void removeLanguageFile(String path)
	{
		langFiles.remove(path);
	}
	
	/**
	 * Returns a Collection of all the LanguageFiles in this LanguagePack.
	 */
	public Collection<LanguageFile> getLanguageFiles()
	{
		return langFiles.values();
	}
	
	/**
	 * Returns the LanguageFile with the specified path.
	 */
	public LanguageFile getLanguageFile(String path)
	{
		return langFiles.get(path);
	}
	
	/**
	 * Returns a LanguagePack containing of only the keys that don't exist in
	 * any of the specified LanguagePacks but exists in this one.
	 */
	public LanguagePack missingKeySubset(Collection<LanguagePack> packs)
	{
		LanguagePack subset = new LanguagePack(language);
		for(LanguageFile langFile : langFiles.values())
		{
			Collection<LanguageFile> uLangFiles =
				new ArrayList<LanguageFile>();
			//Let's get the respective language file from each pack
			for(LanguagePack pack : packs)
			{
				LanguageFile uLangFile =
					pack.getLanguageFile(langFile.getPath());
				//If the language file doesn't even exist in any of the packs,
				//we want to include the whole bunch
				if((uLangFile == null) && !langFile.getPairs().isEmpty())
				{
					subset.addLanguageFile(langFile);
					break;
				}
				uLangFiles.add(uLangFile);
			} 
			
			//Not a very logical sign, but if not all language files were
			//added, that means that means that at least one language file was
			//missing from one of the packs which means that we don't need to
			//do the following:
			if(uLangFiles.size() == packs.size())
			{
				LanguageFile subsetLangFile = 
					langFile.missingKeySubset(uLangFiles);
				if(subsetLangFile != null)
					subset.addLanguageFile(subsetLangFile);
			}
		}
		return subset;
	}
	
	/**
	 * Returns a LanguagePack containing only the keys that exist in this
	 * LanguagePack as well as the specified one. The values and language will
	 * represent the values and language of this LanguagePack.
	 */
	public LanguagePack commonKeySubset(LanguagePack pack)
	{
		LanguagePack commonPack = new LanguagePack(language);
		for(LanguageFile langFile : langFiles.values())
		{
			LanguageFile otherLangFile = pack.getLanguageFile(langFile.getPath());
			if(otherLangFile == null)
				continue;
			
			LanguageFile commonFile = langFile.commonKeySubset(otherLangFile);
			if(!commonFile.getPairs().isEmpty())
				commonPack.addLanguageFile(commonFile);
		}
		return commonPack;
	}
	
	/**
	 * Returns the number of keys summed up from all the LanguageFiles in this
	 * LanguagePack.
	 */
	public int keyCount()
	{
		int keyCount = 0;
		for(LanguageFile langFile : langFiles.values())
			keyCount += langFile.getPairs().size();
		return keyCount;
	}
}