require_relative 'lib/simp/rpm/spec_builder'
require_relative 'lib/simp/rpm/spec_builder_config'

namespaces =[]
config_hash = SIMP::RPM::SpecBuilderConfig.load_config('things_to_build.yaml')
config_hash.each do |proj, dist_configs|
  namespace proj do
    dist_configs.each do |dist_config|
      Dir["#{proj}#{dist_config[:dist]||''}/*.spec"].each do |spec|
        project_dir = File.dirname(spec)
        rpm_dist = project_dir.split('.')[1] || nil
        namespaces << "#{proj}:#{rpm_dist}"
        namespace rpm_dist do
          CLEAN << File.expand_path('dist',File.dirname(spec))
          builder = SIMP::RPM::SpecBuilder.new({proj => dist_config},
                                               File.expand_path(project_dir),
                                               rpm_dist)
          builder.define_rake_tasks spec
        end
      end
    end
  end
end

namespace :pkg do
  dist_dir = File.expand_path('dist',__dir__)
  CLEAN << dist_dir
  desc "builds all projects' RPM packages"
  task :rpm => namespaces.map{|ns| "#{ns}:pkg:rpm" } do
     Dir.chdir __dir__
     mkdir_p dist_dir
     Dir['*/dist/*.rpm'].each{|f| copy(f, dist_dir)}
  end
end
