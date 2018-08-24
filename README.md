# Grunt



grunt.loadNpmTasks('grunt-html2js');
grunt.registerTask('package', [
   .....
   'html2js',
   'concat'
]);


grunt.registerTask('minify', [
  'uglify:distFolder'
]);





//minifier les fichiers js
    uglify: {
      dist: {

        files: {
        'app/componentsJs/jsFiles.min.js': [
          '<%= clientweb.dist %>/components/common/services/*.js',
          '<%= clientweb.dist %>/components/module1/**/*.js',
          '<%= clientweb.dist %>/components/module2/**/*.js',
         ]      
        }
      }
    }
 
//minifier les fichiers HTML et le mettre dans le templatecache
 html2js: {
      options: {

        collapseBooleanAttributes: false,
        collapseWhitespace: false,
        removeAttributeQuotes: false,
        removeComments: false,
        removeEmptyAttributes: false,
        removeRedundantAttributes: false,
        removeScriptTypeAttributes: false,
        removeStyleLinkTypeAttributes: false
      
      },
      main: {
        src: [
          'app/components/module1/page1.html',
          'app/components/module2/page2.html'
         ],
        dest: 'app/componentsJs/templates.js'
      }
    }

	
	
	
//concaténer les js et les HTML .
	
concat: {
  options: {
    separator: ';\n',
    // Replace all 'use strict' statements in the code with a single one at the top
    banner: "'use strict';\n",
    process: function(src, filepath) {
      return '// Source: ' + filepath + '\n' +
      src.replace(/(^|\n)[ \t]*('use strict'|"use strict");?\s*/g, '$1');
    }
  },
  js: {
    src: ['app/componentsJs/templates.js','app/componentsJs/jsFiles.min.js'],
    dest: 'app/livrable/livrable.min.js'
  }


},



//html2js retourne ce templateCache

angular.module("templates-main", ["../app/components/module1/page1.html", "../app/components/module2/page2.html"]);

angular.module("../app/components/module1/page1.html", []).run(["$templateCache", function($templateCache) {
  $templateCache.put("../app/components/module1/page1.html",
    "<div >\n" +
    "\n" +
    "    First Component template (Page 1)\n  {{Controller1.param1}}-{{Controller1.param2}}" +
    "\n" +
    "</div>\n" +
    "\n" +
    "\n" +
    "");
}]);

angular.module("../app/components/module2/page2.html", []).run(["$templateCache", function($templateCache) {
  $templateCache.put("../app/components/module2/page2.html",
    "<div >\n" +
    "\n" +
    "    Second Component template (Page 2)\n" +
    "\n" +
    "</div>\n" +
    "\n" +
    "\n" +
    "");
}]);





//Dans le module de base il faut ajouter le module templates-main qui englobe les fichiers HTML qui sont embarqués dans le templatecache

(function () {
    'use strict';

    angular.module('ComponentsProject', [
            'module1',
			'module2',
            'templates-main'
        ])
        //.config(configAngular)
        .run(runRoute);

    runRoute.$inject = [];

    function runRoute() {
    }

    configAngular.$inject = ["$httpProvider"];

    function configAngular(httpProvider) {
    }


})();



//création du composant FirstDirective
'use strict';

angular.module('module1', []).directive('FirstDirective', function () {
    return {
        templateUrl:'../app/components/module1/page1.html',
        replace: false,
        restrict: 'EA',
        scope: {
            FirstParam: '=FirstParam',
            SecondParam: "=SecondParam"
           
        },
        controller: Controller1,
        controllerAs: 'Controller1'
    }
});

angular.module('module1').controller('Controller1', Controller1);

Controller1.$inject = ['$scope', '$filter', 'Service1', 'Service2',........];

function Controller1(scope, filter, Service1, Service2.........) {

scope.param1=scope.FirstParam;

scope.param2=scope.SecondParam;

}



//création du composant SecondDirective

'use strict';

angular.module('module2', []).directive('SecondDirective', function () {
    return {
        templateUrl:'../app/components/module2/page2.html',
        replace: false,
        restrict: 'EA',
        scope: {
            ThirdParam: '=ThirdParam',
            ForthParam: "=ForthParam"
           
        },
        controller: Controller2,
        controllerAs: 'Controller2'
    }
});

angular.module('module2').controller('Controller2', Controller2);

Controller2.$inject = ['$scope', '$filter', 'Service1', 'Service2',........];

function Controller2(scope, filter, Service1, Service2.........) {

scope.param3=scope.ThirdParam;

scope.param4=scope.ForthParam;

}





//finalement on importe le livrable minifié et on appelle la directive sans fournir les fichiers HTML 


<first-directive first-param="HADDAD" second-param="Riadh" ></first-directive>























