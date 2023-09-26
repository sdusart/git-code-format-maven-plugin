# Changes with Git Code Format Maven Plugin

The plugin iterates on reactor projects to format committed files if they are in the sources directories.
It needs to be executed by Maven for each reactor projects, which can be really slow with many reactor projects.

Modifications reside in the com.cosium.code.format.OnPreCommitMojo.isFormattable method, files are eligible for
formatting if their absolute path contains 'src/main/java' or 'src/test/java'.
This way, the plugin can be run only on the top level project and but formats all changed java files found in
the 'standard' directories.                                                        

```xml
<build>
  <plugins>
    <plugin>
      <groupId>com.cosium.code</groupId>
      <artifactId>git-code-format-maven-plugin</artifactId>
      <version>${git-code-format-maven-plugin.version}</version>
      <executions>
        <!-- ... -->
      </executions>
      <configuration>
        <!-- ... -->
        <preCommitHookPipeline>-N</preCommitHookPipeline>
      </configuration>
    </plugin>
  </plugins>
</build>
```

This is a quick and dirty hack but its saves a lot of time on each commit on a project with ~100 poms.
