# Jenkins to GitHub Actions Migration - Demo Repository

**Complete 60-minute presentation with 5 continuous demos**

This repository contains everything you need to deliver a comprehensive presentation on migrating Jenkins pipelines to GitHub Actions using the GitHub Actions Importer.

## 📚 What's Included

### Main Documentation

1. **[DEMO_GUIDE.md](DEMO_GUIDE.md)** - Complete demo guide with all 5 demos
   - Demo 1: Setup & Configuration (10 min)
   - Demo 2: Audit Your Pipelines (10 min)
   - Demo 3: Forecast Usage & Costs (10 min)
   - Demo 4: Dry Run Migration (12 min)
   - Demo 5: Execute Full Migration (13 min)

2. **[PRESENTER_SCRIPT.md](PRESENTER_SCRIPT.md)** - Word-for-word presenter script
   - Exact timing for each section
   - Talking points and transitions
   - Audience engagement prompts
   - Q&A preparation

3. **[QUICK_REFERENCE.md](QUICK_REFERENCE.md)** - Quick command reference
   - All essential commands
   - Common options and flags
   - Environment variable setup
   - Jenkinsfile to GitHub Actions mapping

4. **[TROUBLESHOOTING.md](TROUBLESHOOTING.md)** - Troubleshooting guide
   - Common issues and solutions
   - Emergency rollback procedures
   - Performance optimization tips

### Sample Materials

5. **`demos/` directory** - Sample Jenkinsfiles for demonstrations
   - `demo4-simple-pipeline/` - Basic Node.js pipeline
   - `demo4-complex-pipeline/` - Advanced pipeline with parallel stages
   - `demo5-monorepo/` - Monorepo with multiple services

## 🚀 Quick Start for Presenters

### Before Your Presentation

1. **Install Prerequisites** (15 minutes)
   ```bash
   # Install GitHub CLI
   brew install gh  # macOS
   # or follow: https://cli.github.com/

   # Install Docker Desktop
   # Download from: https://www.docker.com/products/docker-desktop

   # Install GitHub Actions Importer
   gh extension install github/gh-actions-importer
   ```

2. **Prepare Credentials** (5 minutes)
   - Generate Jenkins API token
   - Generate GitHub Personal Access Token (with `workflow` scope)
   - Have both ready to paste during Demo 1

3. **Test Your Setup** (10 minutes)
   ```bash
   # Verify installations
   docker --version
   gh --version
   gh actions-importer version

   # Optional: Pre-configure to save time during presentation
   gh actions-importer configure
   ```

4. **Review Materials** (30 minutes)
   - Read [DEMO_GUIDE.md](DEMO_GUIDE.md) thoroughly
   - Familiarize yourself with [PRESENTER_SCRIPT.md](PRESENTER_SCRIPT.md)
   - Practice timing with each demo
   - Bookmark [QUICK_REFERENCE.md](QUICK_REFERENCE.md) for live reference

### During Your Presentation

1. **Follow the timeline** in [PRESENTER_SCRIPT.md](PRESENTER_SCRIPT.md)
2. **Use [QUICK_REFERENCE.md](QUICK_REFERENCE.md)** if you need command help
3. **Have [TROUBLESHOOTING.md](TROUBLESHOOTING.md)** open in case of issues
4. **Show sample Jenkinsfiles** from the `demos/` directory

### After Your Presentation

- Share this repository with attendees
- Provide links to official documentation
- Offer to help with initial migrations

## 📋 Presentation Structure

| Time | Section | Duration | Type |
|------|---------|----------|------|
| 00:00-05:00 | Introduction & Why Migrate | 5 min | Slides |
| 05:00-15:00 | Demo 1: Setup & Configuration | 10 min | Live Demo |
| 15:00-25:00 | Demo 2: Audit Your Pipelines | 10 min | Live Demo |
| 25:00-35:00 | Demo 3: Forecast Usage & Costs | 10 min | Live Demo |
| 35:00-47:00 | Demo 4: Dry Run Migration | 12 min | Live Demo |
| 47:00-60:00 | Demo 5: Execute Full Migration | 13 min | Live Demo |
| 60:00-65:00 | Summary & Q&A | 5 min | Slides + Discussion |

## 🎯 Target Audience

- **Jenkins Experience:** Expert level
- **GitHub Actions Experience:** Beginner level
- **Role:** DevOps Engineers, Platform Engineers, Engineering Managers
- **Goal:** Learn to migrate Jenkins pipelines automatically

## 🔗 Official Resources

- [GitHub Actions Importer Documentation](https://docs.github.com/en/actions/tutorials/migrate-to-github-actions/automated-migrations/use-github-actions-importer)
- [Jenkins Migration Guide](https://docs.github.com/en/actions/tutorials/migrate-to-github-actions/automated-migrations/jenkins-migration)
- [GitHub Actions Documentation](https://docs.github.com/actions)
- [GitHub Actions Marketplace](https://github.com/marketplace?type=actions)

## 💡 Key Takeaways

By the end of the presentation, attendees will be able to:

✅ Install and configure GitHub Actions Importer  
✅ Audit their Jenkins environment to understand migration scope  
✅ Forecast GitHub Actions costs based on historical usage  
✅ Perform dry-run migrations to preview output  
✅ Execute full migrations with automatic PR creation  
✅ Validate migrated workflows  
✅ Troubleshoot common migration issues  

## 📊 Expected Migration Results

Based on typical Jenkins environments:

- **Automatic Conversion Rate:** 70-95% (declarative pipelines)
- **Time Savings:** 3-4 weeks vs manual migration
- **Cost Reduction:** 40-60% vs self-hosted Jenkins
- **Build Time Improvement:** 30-40% faster with caching
- **Maintenance Reduction:** 80% less time on CI/CD maintenance

## 🛠️ Customization Tips

### For Your Organization

1. **Update examples** with your company's actual pipelines
2. **Adjust timing** based on audience technical level
3. **Add specific cost numbers** from your Jenkins environment
4. **Include success stories** from early adopters in your org
5. **Customize troubleshooting** with known issues in your setup

### For Different Audiences

- **For Managers:** Focus more on Demo 3 (costs) and ROI
- **For Engineers:** Deep dive into Demo 4 & 5 (technical details)
- **For Security:** Add security scanning and compliance topics
- **For Enterprise:** Include discussion of self-hosted runners and GitHub Enterprise features

## 🤝 Contributing

Found an issue or want to improve the demos? Contributions welcome!

1. Fork the repository
2. Make your improvements
3. Test the demos thoroughly
4. Submit a pull request

## 📝 License

This repository is provided as-is for educational and demonstration purposes.

## 📞 Support

For questions about:
- **This presentation:** Open an issue in this repository
- **GitHub Actions Importer:** Visit [GitHub Community](https://github.community/)
- **GitHub Actions:** See [GitHub Docs](https://docs.github.com/actions)

---

## Quick Command Reference

```bash
# Installation
gh extension install github/gh-actions-importer

# Configuration
gh actions-importer configure

# Audit
gh actions-importer audit jenkins --output-dir ./audit

# Forecast
gh actions-importer forecast jenkins --output-dir ./forecast

# Dry Run
gh actions-importer dry-run jenkins \
  --source-url http://jenkins/job/my-job \
  --output-dir ./dry-run

# Migrate
gh actions-importer migrate jenkins \
  --source-url http://jenkins/job/my-job \
  --target-url https://github.com/org/repo \
  --output-dir ./migration
```

---

**Good luck with your presentation! 🎉**

Remember: Start small, test thoroughly, and run in parallel. Migration is a journey, not a sprint!