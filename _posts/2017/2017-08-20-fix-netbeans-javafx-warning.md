---
title: Fix the javafx Warning
---

# 警告信息

> Missing Codebase manifest attribute for:xxx.jar

you can edit your build.xml and add the following ANT targets and macros:

    <target name="-post-jfx-jar">
        <update-libs-manifest />
        <update-jar-manifest jarfile="${jfx.deployment.dir}${file.separator}${jfx.deployment.jar}" />
    </target>   

    <macrodef name="update-libs-manifest">
        <sequential>
            <property name="pp_rebase_dir" value="${basedir}${file.separator}${dist.dir}${file.separator}lib"/>
            <property name="pp_rebase_fs" value="*.jar"/>

            <condition property="manifest.codebase" value="${manifest.custom.codebase}" else="*">
                <and>
                    <isset property="manifest.custom.codebase" />
                    <not>
                        <equals arg1="${manifest.custom.codebase}" arg2="" trim="true" />
                    </not>
                </and>
            </condition>

            <condition property="manifest.permissions" value="all-permissions" else="sandbox">
                <isset property="permissions-elevated" />
            </condition>

            <echo message="Updating libraries manifest => Codebase: ${manifest.codebase} / Permissions: ${manifest.permissions}" />
            <script language="javascript">
                <![CDATA[
                    var dir = project.getProperty("pp_rebase_dir");
                    var fDir = new java.io.File(dir);
                    if( fDir.exists() ) {
                        var callTask = project.createTask("antcall");
                        callTask.setTarget("-update-jar-macro-call");
                        var param = callTask.createParam();
                        param.setName("jar.file.to.rebase");
                        var includes = project.getProperty("pp_rebase_fs");
                        var fs = project.createDataType("fileset");
                        fs.setDir( fDir );
                        fs.setIncludes(includes);
                        var ds = fs.getDirectoryScanner(project);
                        var srcFiles = ds.getIncludedFiles();
                        for (i=0; i<srcFiles.length; i++) {
                            param.setValue(dir + "${file.separator}" + srcFiles[i]);
                            callTask.perform();
                        }
                    }
                ]]>
            </script>
        </sequential>
    </macrodef>

    <target name="-update-jar-macro-call">
        <update-jar-manifest jarfile="${jar.file.to.rebase}"/>
    </target>

    <macrodef name="update-jar-manifest">
        <attribute name="jarfile"/>
        <sequential>
            <echo message="jarfile = @{jarfile}" />
            <jar file="@{jarfile}" update="true">
                <manifest>
                    <attribute name="Codebase" value="${manifest.codebase}"/>
                    <attribute name="Permissions" value="${manifest.permissions}"/>
                </manifest>
            </jar>
        </sequential>
    </macrodef>

After that, you just have to add manifest.custom.codebase=<your_codebase> to your project.properties. For example: manifest.custom.codebase=localhost

Attention: This code is for a JavaFX application using Netbeans default ANT build process. You'll need to update it accordingly if that's not your case - that will require a change on the first target (<target name="-post-jfx-jar">), the property names and the condition that checks the permissions-elevated property.