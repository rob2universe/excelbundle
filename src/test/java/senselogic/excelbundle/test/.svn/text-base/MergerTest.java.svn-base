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

package senselogic.excelbundle.test;

import java.io.File;
import java.io.IOException;

import senselogic.excelbundle.LanguageFile;
import senselogic.excelbundle.LanguagePack;
import senselogic.excelbundle.LanguageTreeIO;
import senselogic.excelbundle.Merger;
import junit.framework.TestCase;

/**
 * TestCase for Merger.
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class MergerTest extends TestCase
{
	private File root = new File("test/classes/");
	
	public void testMerge() throws IOException
	{
		try
		{
			LanguagePack en = new LanguagePack("en");
			
			LanguageFile langFile1 = new LanguageFile("/foobar1", "en");
			langFile1.setValue("foo1", "bar1");
			langFile1.setValue("foo2", "bar3");
			langFile1.setValue("foo3", "bar4");
			en.addLanguageFile(langFile1);
			
			LanguageFile langFile2 = new LanguageFile("/foobar2", "en");
			langFile2.setValue("foo1", "bar1");
			langFile2.setValue("foo2", "bar3");
			langFile2.setValue("foo3", "bar4");
			en.addLanguageFile(langFile2);
			
			LanguagePack sv = new LanguagePack("sv");
			
			langFile1 = new LanguageFile("/foobar1", "sv");
			langFile1.setValue("foo1", "bar1");
			langFile1.setValue("foo2", "bar3");
			langFile1.setValue("foo3", "bar4");
			sv.addLanguageFile(langFile1);
			
			LanguagePack expect = new LanguagePack("sv");
			
			langFile1 = new LanguageFile("/foobar1", "sv");
			langFile1.setValue("foo1", "bar1");
			langFile1.setValue("foo2", "bar2");
			langFile1.setValue("foo3", "bar3");
			expect.addLanguageFile(langFile1);
			
			langFile2 = new LanguageFile("/foobar2", "sv");
			langFile2.setValue("foo1", "bar1");
			langFile2.setValue("foo2", "bar3");
			langFile2.setValue("foo3", "bar4");
			expect.addLanguageFile(langFile2);
			
			LanguageTreeIO tree = new LanguageTreeIO(root);
			tree.save(en.getLanguageFile("/foobar1"));
			tree.save(en.getLanguageFile("/foobar2"));
			tree.save(sv.getLanguageFile("/foobar1"));
			Merger merger = new Merger(tree, en);
			merger.merge(expect, false);
			
			assertTrue(merger.getFilesChanged() == 1);
			assertTrue(merger.getKeysChanged() == 2);
			assertTrue(merger.getFilesCreated() == 1);
			assertTrue(merger.getKeysAdded() == 3);
			
			tree.update();
			
			LanguagePack result = tree.loadLanguage("sv");
			assertTrue(result.getLanguageFiles().size() ==
				expect.getLanguageFiles().size());
			for(LanguageFile langFile : expect.getLanguageFiles())
			{
				assertFalse(langFile.differs(
						result.getLanguageFile(langFile.getPath())));
			}
		}
		finally
		{
			new File("test/classes/foobar1_sv.properties").delete();
			new File("test/classes/foobar2_sv.properties").delete();
			new File("test/classes/foobar1_en.properties").delete();
			new File("test/classes/foobar2_en.properties").delete();
		}
	}
}