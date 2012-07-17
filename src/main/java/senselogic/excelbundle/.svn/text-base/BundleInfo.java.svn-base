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

import java.util.Collection;

/**
 * Contains information about a resource bundle.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class BundleInfo
{
    // Attributes ----------------------------------------------------
    private String path;
    private Collection<String> languages;

    // Constructors --------------------------------------------------

    /**
     * Constructs a new BundleInfo.
     *
     * @param path       the path to the bundle
     * @param languages  the languages available for this bundle
     */
    public BundleInfo(String path, Collection<String> languages)
    {
        this.path = path;
        this.languages = languages;
    }

    // Public --------------------------------------------------------
    
    /**
     * Returns a list of the languages available for this bundle.
     */
	public Collection<String> getLanguages()
	{
		return languages;
	}

	/**
	 * Returns the logical path of this bundle relative to the root of the
	 * tree.
	 */
	public String getPath()
	{
		return path;
	}
}