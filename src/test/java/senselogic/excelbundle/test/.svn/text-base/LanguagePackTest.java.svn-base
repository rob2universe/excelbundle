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

import java.util.ArrayList;
import java.util.Collection;

import senselogic.excelbundle.LanguageFile;
import senselogic.excelbundle.LanguagePack;
import junit.framework.TestCase;

/**
 * TestCase for LanguagePack
 *
 * @author Emil Eriksson
 * @version $Revision$
 */
public class LanguagePackTest extends TestCase
{
	public void testMissingKeySubset()
	{
		LanguagePack ref = new LanguagePack("en");
		
		LanguageFile langFile1 = new LanguageFile("/foobar1", "en");
		langFile1.setValue("foo1", "bar1");
		langFile1.setValue("foo2", "bar2");
		langFile1.setValue("foo3", "bar3");
		langFile1.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile1);
		
		LanguageFile langFile2 = new LanguageFile("/foobar2", "en");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo2", "bar2");
		langFile2.setValue("foo3", "bar3");
		langFile2.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile2);
		
		LanguageFile langFile3 = new LanguageFile("/foobar3", "en");
		langFile3.setValue("foo1", "bar1");
		langFile3.setValue("foo2", "bar2");
		langFile3.setValue("foo3", "bar3");
		langFile3.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile3);
		
		LanguageFile langFile4 = new LanguageFile("/foobar4", "en");
		langFile4.setValue("foo1", "bar1");
		langFile4.setValue("foo2", "bar2");
		langFile4.setValue("foo3", "bar3");
		langFile4.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile4);
		
		LanguagePack sv = new LanguagePack("sv");
		
		langFile1 = new LanguageFile("/foobar1", "sv");
		langFile1.setValue("foo1", "bar1");
		langFile1.setValue("foo3", "bar3");
		sv.addLanguageFile(langFile1);
		
		langFile3 = new LanguageFile("/foobar3", "sv");
		langFile3.setValue("foo1", "bar1");
		langFile3.setValue("foo2", "bar2");
		langFile3.setValue("foo3", "bar3");
		langFile3.setValue("foo4", "bar4");
		sv.addLanguageFile(langFile3);
		
		langFile4 = new LanguageFile("/foobar4", "sv");
		langFile4.setValue("foo1", "bar1");
		langFile4.setValue("foo2", "bar2");
		langFile4.setValue("foo3", "bar3");
		langFile4.setValue("foo4", "bar4");
		sv.addLanguageFile(langFile4);
		
		LanguagePack no = new LanguagePack("no");
		
		langFile1 = new LanguageFile("/foobar1", "no");
		langFile1.setValue("foo1", "bar1");
		langFile1.setValue("foo2", "bar2");
		no.addLanguageFile(langFile1);
		
		langFile2 = new LanguageFile("/foobar2", "no");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo2", "bar2");
		langFile2.setValue("foo3", "bar3");
		langFile2.setValue("foo4", "bar4");
		no.addLanguageFile(langFile2);
		
		langFile4 = new LanguageFile("/foobar4", "no");
		langFile4.setValue("foo1", "bar1");
		langFile4.setValue("foo2", "bar2");
		langFile4.setValue("foo3", "bar3");
		langFile4.setValue("foo4", "bar4");
		no.addLanguageFile(langFile4);
		
		LanguagePack expect = new LanguagePack("en");
		
		langFile1 = new LanguageFile("/foobar1", "en");
		langFile1.setValue("foo2", "bar2");
		langFile1.setValue("foo3", "bar3");
		langFile1.setValue("foo4", "bar4");
		expect.addLanguageFile(langFile1);
		
		langFile2 = new LanguageFile("/foobar2", "en");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo2", "bar2");
		langFile2.setValue("foo3", "bar3");
		langFile2.setValue("foo4", "bar4");
		expect.addLanguageFile(langFile2);
		
		langFile3 = new LanguageFile("/foobar3", "en");
		langFile3.setValue("foo1", "bar1");
		langFile3.setValue("foo2", "bar2");
		langFile3.setValue("foo3", "bar3");
		langFile3.setValue("foo4", "bar4");
		expect.addLanguageFile(langFile3);
		
		Collection<LanguagePack> packs = new ArrayList<LanguagePack>();
		packs.add(sv);
		packs.add(no);
		
		LanguagePack result = ref.missingKeySubset(packs);
		assertTrue(
				result.getLanguageFiles().size() == 
					expect.getLanguageFiles().size());
		for(LanguageFile langFile : expect.getLanguageFiles())
		{
			assertFalse(langFile.differs(
					result.getLanguageFile(langFile.getPath())));
		}
	}
	
	public void testCommonKeySubset()
	{
		LanguagePack ref = new LanguagePack("en");
		
		LanguageFile langFile1 = new LanguageFile("/foobar1", "en");
		langFile1.setValue("foo1", "bar1");
		langFile1.setValue("foo2", "bar2");
		langFile1.setValue("foo3", "bar3");
		langFile1.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile1);
		
		LanguageFile langFile2 = new LanguageFile("/foobar2", "en");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo2", "bar2");
		langFile2.setValue("foo3", "bar3");
		langFile2.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile2);
		
		LanguageFile langFile3 = new LanguageFile("/foobar3", "en");
		langFile3.setValue("foo1", "bar1");
		langFile3.setValue("foo2", "bar2");
		langFile3.setValue("foo3", "bar3");
		langFile3.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile3);
		
		LanguageFile langFile4 = new LanguageFile("/foobar4", "en");
		langFile4.setValue("foo1", "bar1");
		langFile4.setValue("foo2", "bar2");
		langFile4.setValue("foo3", "bar3");
		langFile4.setValue("foo4", "bar4");
		ref.addLanguageFile(langFile4);
		
		LanguagePack pack = new LanguagePack("en");
		
		langFile1 = new LanguageFile("/foobar1", "en");
		langFile1.setValue("foo2", "bar2");
		langFile1.setValue("foo3", "bar3");
		langFile1.setValue("foo4", "bar4");
		pack.addLanguageFile(langFile1);
		
		langFile2 = new LanguageFile("/foobar2", "en");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo2", "bar2");
		langFile2.setValue("foo3", "bar3");
		langFile2.setValue("foo4", "bar4");
		pack.addLanguageFile(langFile2);
		
		langFile3 = new LanguageFile("/foobar3", "en");
		langFile3.setValue("foo1", "bar1");
		langFile3.setValue("foo2", "bar2");
		langFile3.setValue("foo3", "bar3");
		langFile3.setValue("foo4", "bar4");
		pack.addLanguageFile(langFile3);
		
		LanguagePack expect = new LanguagePack("en");
		
		langFile1 = new LanguageFile("/foobar1", "en");
		langFile1.setValue("foo2", "bar2");
		langFile1.setValue("foo3", "bar3");
		langFile1.setValue("foo4", "bar4");
		expect.addLanguageFile(langFile1);
		
		langFile2 = new LanguageFile("/foobar2", "en");
		langFile2.setValue("foo1", "bar1");
		langFile2.setValue("foo2", "bar2");
		langFile2.setValue("foo3", "bar3");
		langFile2.setValue("foo4", "bar4");
		expect.addLanguageFile(langFile2);
		
		langFile3 = new LanguageFile("/foobar3", "en");
		langFile3.setValue("foo1", "bar1");
		langFile3.setValue("foo2", "bar2");
		langFile3.setValue("foo3", "bar3");
		langFile3.setValue("foo4", "bar4");
		expect.addLanguageFile(langFile3);
		
		LanguagePack result = pack.commonKeySubset(ref);
		
		assertTrue(result.getLanguageFiles().size() ==
			expect.getLanguageFiles().size());
		for(LanguageFile langFile : expect.getLanguageFiles())
		{
			assertFalse(langFile.differs(
					result.getLanguageFile(langFile.getPath())));
		}
	}
}