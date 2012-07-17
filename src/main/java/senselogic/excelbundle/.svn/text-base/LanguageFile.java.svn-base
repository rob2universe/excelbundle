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
import java.util.Map;
import java.util.LinkedHashMap;
import java.util.Collection;

/**
 * Class for storing data from a language file.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class LanguageFile
{
    // Static --------------------------------------------------------
    public final static String DEFAULT_LANGUAGE = "default";

    // Attributes ----------------------------------------------------
    private String path;
    private String language;
    private Map<String, KeyValuePair> pairs =
    	new LinkedHashMap<String, KeyValuePair>();

    // Constructors --------------------------------------------------
    /**
     * Constructs a new LanguageFile.
     *
     * @param path      the logical path to the resource bundle
     * @param language  the language of the file
     */
    public LanguageFile(String path, String language)
    {
        this.path = path;
        this.language = language;
    }
    
    /**
     * Constructs a new LanguageFile which is a copy of another one.
     */
    public LanguageFile(LanguageFile langFile)
    {
    	this(langFile.getPath(), langFile.getLanguage());
    	for(KeyValuePair pair : langFile.getPairs())
    		setValue(pair.getKey(), pair.getValue());
    }

    // Public --------------------------------------------------------
    /**
     * Returns the path of the bundle containing this language file relative to
     * the root of the source tree.
     */
    public String getPath()
    {
        return path;
    }

    /**
     * Returns the language code.
     */
    public String getLanguage()
    {
        return language;
    }

    /**
     * Returns the KeyValuePair with the specified key.
     */
    public KeyValuePair getPair(String key)
    {
        return pairs.get(key);
    }

    /**
     * Returns a Collection of all the KeyValuePairs in this LanguageFile.
     */
    public Collection<KeyValuePair> getPairs()
    {
        return pairs.values();
    }
    
    /**
     * Removes the KeyValuePair with the specified key.
     */
    public void removeKeyValuePair(String key)
    {
    	pairs.remove(key);
    }

    /**
     * Sets the value of the specified KeyValuePair and creates it if it 
     * doesn't exist already. If null or an empty or whitespace-only string is
     * passed, the KeyValuePair is removed.
     *
     * @param key    the key to set
     * @param value  the value to set
     */
    public void setValue(String key, String value)
    {
    	if(value != null)
    		value = value.trim();
    	
    	if((value == null) || (value.length() == 0))
    	{
    		pairs.remove(key);
    		return;
    	}
    	
        KeyValuePair pair = pairs.get(key);
        if(pair == null)
        {
            pairs.put(key, new KeyValuePair(key, value));
            return;
        }
        pair.setValue(value);
    }
    
    /**
     * Returns the value of the specified KeyValuePair or null if it doesn't
     * exist.
     */
    public String getValue(String key)
    {
    	KeyValuePair pair = pairs.get(key);
    	if(pair == null)
    		return null;
    	
    	return pair.getValue();
    }

    /**
     * Returns true if any of the values in the specified LanguageFile differs
     * from the values is this one.
     */
    public boolean differs(LanguageFile file)
    {
    	if(file.getPairs().size() != pairs.size())
            return true;
    	
        for(KeyValuePair pair1 : pairs.values())
        {
            KeyValuePair pair2 = file.getPair(pair1.getKey());
            if(pair2 == null)
                return true;
            if(!pair1.getValue().equals(pair2.getValue()))
                return true;
        }
        return false;
    }

    /**
     * Returns true if specified LanguageFile has the same keys as this one.
     */
    public boolean hasSameKeys(LanguageFile file)
    {
        if(file.getPairs().size() != pairs.size())
            return false;

        for(KeyValuePair pair : pairs.values())
        {
            if(file.getPair(pair.getKey()) == null)
                return false;
        }
        return true;
    }

    /**
     * Merges the specified LanguageFile with this one and returns it as a
     * copy.
     *
     * @param file  the file to merge
     * @param add   whether keys that don't exist in this LanguageFile should
     *              be added or not if the exist in the specified one
     */
    public LanguageFile merge(LanguageFile file, boolean add)
    {
    	LanguageFile newFile = new LanguageFile(this);
    	for(KeyValuePair pair : file.getPairs())
    	{
    		if((newFile.getPair(pair.getKey()) == null) && !add)
    			continue;
    		
    		newFile.setValue(pair.getKey(), pair.getValue());
    	}
    	return newFile;
    }
    
    public LanguageFile missingKeySubset(LanguageFile langFile)
    {
    	Collection<LanguageFile> c = new ArrayList<LanguageFile>(1);
    	c.add(langFile);
    	return missingKeySubset(c);
    }
    
    /**
     * Returns a LanguageFile containing only the keys from this LanguageFile 
     * that do not exist in any of the specified LanguageFiles. If everything
     * exists everywhere, null is returned.
     */
    public LanguageFile missingKeySubset(Collection<LanguageFile> langFiles)
    {
    	LanguageFile subset = new LanguageFile(path, language);
    	for(KeyValuePair pair : pairs.values())
    	{
    		boolean translated = true;
    		for(LanguageFile langFile : langFiles)
    		{
    			String value = langFile.getValue(pair.getKey());
    			if(value == null)
    			{
    				translated = false;
    				break;
    			}
    		}
    		if(!translated)
    			subset.setValue(pair.getKey(), pair.getValue());
    	}
    	if(subset.getPairs().isEmpty())
    		return null;
    	
    	return subset;
    }
    
    /**
     * Returns a LanguageFile similar to this one containing only the keys 
     * which exist in the specified LanguageFile as well.
     */
    public LanguageFile commonKeySubset(LanguageFile langFile)
    {
    	LanguageFile result = new LanguageFile(path, language);
    	for(KeyValuePair pair: langFile.getPairs())
    	{
    		if(pairs.containsKey(pair.getKey()))
    			result.setValue(pair.getKey(), getValue(pair.getKey()));
    	}
    	return result;
    }
    
    /**
     * Returns the filename and path of this LanguageFile relative to the root
     * of the source tree.
     */
    public String getFilename()
    {
    	return getFilename(path, language);
    }
    
    /**
     * Returns the filename and path that a file with the specified path and
     * language would have.
     */
    public static String getFilename(String path, String language)
    {
    	if(language.equals(DEFAULT_LANGUAGE))
    		return path + ".properties";
    	else
    		return path + "_" + language + ".properties";
    }
    
    // Object overrides ----------------------------------------------
    public String toString()
    {
    	StringBuilder buf = new StringBuilder("[lang=");
    	buf.append(language);
    	buf.append(" path=");
    	buf.append(path);
    	buf.append(" values(");
    	for(KeyValuePair pair : pairs.values())
    	{
    		buf.append(pair.getKey());
    		buf.append("=");
    		buf.append(pair.getValue());
    		buf.append(",");
    	}
    	buf.append(")]");
    	return buf.toString();
    }

    // Inner classes -------------------------------------------------
    public class KeyValuePair
    {
        // Attributes ----------------------------------------------------
        private String key;
        private String value;
        
        // Constructors --------------------------------------------------
        private KeyValuePair(String key, String value)
        {
            this.key = key;
            this.value = value;
        }

        // Public --------------------------------------------------------
        public String getKey()
        {
            return key;
        }

        public String getValue()
        {
            return value;
        }

        // Private -------------------------------------------------------
        public void setValue(String value)
        {
            this.value = value;
        }
    }
}