### ProjectDependency Graph 만들기
</hr>

1. 프로젝트의 gradle에 다음 .gradle 코드를 포함한 파일을 만든다.
 - Todo : 해당 코드 분석해보기
```
// from: https://github.com/JakeWharton/SdkSearch/blob/3351cad9bfacb0a364858e843774147143f58c7a/gradle/projectDependencyGraph.gradle
task projectDependencyGraph {
    doLast {
        def dot = new File(rootProject.buildDir, 'reports/dependency-graph/project.dot')
        dot.parentFile.mkdirs()
        dot.delete()

        dot << 'digraph {\n'
        dot << "  graph [label=\"${rootProject.name}\\n \",labelloc=t,fontsize=30,ranksep=1.4];\n"
        dot << '  node [style=filled, fillcolor="#bbbbbb"];\n'
        dot << '  rankdir=TB;\n'

        def rootProjects = []
        def queue = [rootProject]
        while (!queue.isEmpty()) {
            def project = queue.remove(0)
            rootProjects.add(project)
            queue.addAll(project.childProjects.values())
        }

        def projects = new LinkedHashSet<Project>()
        def dependencies = new LinkedHashMap<Tuple2<Project, Project>, List<String>>()
        def multiplatformProjects = []
        def jsProjects = []
        def androidProjects = []
        def javaProjects = []

        queue = [rootProject]
        while (!queue.isEmpty()) {
            def project = queue.remove(0)
            queue.addAll(project.childProjects.values())

            if (project.plugins.hasPlugin('org.jetbrains.kotlin.multiplatform')) {
                multiplatformProjects.add(project)
            }
            if (project.plugins.hasPlugin('kotlin2js')) {
                jsProjects.add(project)
            }
            if (project.plugins.hasPlugin('com.android.library') || project.plugins.hasPlugin('com.android.application')) {
                androidProjects.add(project)
            }
            if (project.plugins.hasPlugin('java-library') || project.plugins.hasPlugin('java')) {
                javaProjects.add(project)
            }

            project.configurations.all { config ->
                config.dependencies
                        .withType(ProjectDependency)
                        .collect { it.dependencyProject }
                        .each { dependency ->
                            projects.add(project)
                            projects.add(dependency)
                            rootProjects.remove(dependency)

                            def graphKey = new Tuple2<Project, Project>(project, dependency)
                            def traits = dependencies.computeIfAbsent(graphKey) { new ArrayList<String>() }

                            if (config.name.toLowerCase().endsWith('implementation')) {
                                traits.add('style=dotted')
                            }
                        }
            }
        }

        projects = projects.sort { it.path }

        dot << '\n  # Projects\n\n'
        for (project in projects) {
            def traits = []

            if (rootProjects.contains(project)) {
                traits.add('shape=box')
            }

            if (multiplatformProjects.contains(project)) {
                traits.add('fillcolor="#ffd2b3"')
            } else if (jsProjects.contains(project)) {
                traits.add('fillcolor="#ffffba"')
            } else if (androidProjects.contains(project)) {
                traits.add('fillcolor="#baffc9"')
            } else if (javaProjects.contains(project)) {
                traits.add('fillcolor="#ffb3ba"')
            } else {
                traits.add('fillcolor="#eeeeee"')
            }

            dot << "  \"${project.path}\" [${traits.join(", ")}];\n"
        }

        dot << '\n  {rank = same;'
        for (project in projects) {
            if (rootProjects.contains(project)) {
                dot << " \"${project.path}\";"
            }
        }
        dot << '}\n'

        dot << '\n  # Dependencies\n\n'
        dependencies.forEach { key, traits ->
            dot << "  \"${key.first.path}\" -> \"${key.second.path}\""
            if (!traits.isEmpty()) {
                dot << " [${traits.join(", ")}]"
            }
            dot << '\n'
        }

        dot << '}\n'

        def p = 'dot -Tpng -O project.dot'.execute([], dot.parentFile)
        p.waitFor()
        if (p.exitValue() != 0) {
            throw new RuntimeException(p.errorStream.text)
        }

        println("Project module dependency graph created at ${dot.absolutePath}.png")
    }
}

```

2. Project 수준의 build.gradle.kts에 위 파일 사용을 위한 코드를 추가해준다
```
apply{
    from("gradle/projectDependencyGraph.gradle")
}
```

3. terminal에서 다음 명령어를 실행한다.
```
./gradlew projectDependencyGraph
```

4. /build/reports/dependency-graph 에 추가된 파일을 확인한다
<img width="369" alt="image" src="https://github.com/user-attachments/assets/1b19303c-dde5-41c1-9ef5-e33e6f2e92a1" />
