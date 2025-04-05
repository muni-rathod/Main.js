# Main.js
// Tools data - this would contain all 100+ tools
const toolsData = [
    {
        id: 1,
        name: "Image to PNG Converter",
        description: "Convert any image format to PNG",
        category: "image",
        url: "tools/image/image-to-png.html",
        icon: "bi bi-image"
    },
    {
        id: 2,
        name: "Image to JPG Converter",
        description: "Convert any image format to JPG",
        category: "image",
        url: "tools/image/image-to-jpg.html",
        icon: "bi bi-image"
    },
    // ... all other tools would be listed here
    {
        id: 100,
        name: "Name to Numerology Calculator",
        description: "Calculate numerology numbers from names",
        category: "misc",
        url: "tools/misc/name-numerology.html",
        icon: "bi bi-123"
    }
];

// Initialize the page
document.addEventListener('DOMContentLoaded', function() {
    // Load all tools initially
    displayTools(toolsData);
    
    // Set up category filters
    const categoryButtons = document.querySelectorAll('.category-btn');
    categoryButtons.forEach(button => {
        button.addEventListener('click', function() {
            const category = this.getAttribute('data-category');
            filterToolsByCategory(category);
            
            // Update active button
            categoryButtons.forEach(btn => btn.classList.remove('active'));
            this.classList.add('active');
            
            // Update category title
            const categoryTitle = document.getElementById('current-category');
            if (category === 'all') {
                categoryTitle.textContent = 'All Tools';
            } else {
                categoryTitle.textContent = `${this.textContent} Tools`;
            }
        });
    });
    
    // Set up search functionality
    const searchInput = document.getElementById('tool-search');
    const searchBtn = document.getElementById('search-btn');
    
    searchBtn.addEventListener('click', performSearch);
    searchInput.addEventListener('keyup', function(e) {
        if (e.key === 'Enter') {
            performSearch();
        }
    });
});

function displayTools(tools) {
    const toolsContainer = document.getElementById('tools-container');
    toolsContainer.innerHTML = '';
    
    tools.forEach(tool => {
        const toolCard = document.createElement('div');
        toolCard.className = 'col-md-4 col-lg-3 mb-4';
        toolCard.innerHTML = `
            <div class="card h-100 tool-card">
                <div class="card-body">
                    <div class="tool-icon mb-3">
                        <i class="${tool.icon}"></i>
                    </div>
                    <h5 class="card-title">${tool.name}</h5>
                    <p class="card-text">${tool.description}</p>
                    <a href="${tool.url}" class="btn btn-primary">Use Tool</a>
                </div>
            </div>
        `;
        toolsContainer.appendChild(toolCard);
    });
}

function filterToolsByCategory(category) {
    if (category === 'all') {
        displayTools(toolsData);
        return;
    }
    
    const filteredTools = toolsData.filter(tool => tool.category === category);
    displayTools(filteredTools);
}

function performSearch() {
    const searchTerm = document.getElementById('tool-search').value.toLowerCase();
    const searchResults = document.getElementById('search-results');
    
    if (!searchTerm) {
        searchResults.innerHTML = '';
        displayTools(toolsData);
        return;
    }
    
    const filteredTools = toolsData.filter(tool => 
        tool.name.toLowerCase().includes(searchTerm) || 
        tool.description.toLowerCase().includes(searchTerm)
    );
    
    if (filteredTools.length === 0) {
        searchResults.innerHTML = '<p class="text-danger">No tools found matching your search.</p>';
        document.getElementById('tools-container').innerHTML = '';
    } else {
        searchResults.innerHTML = `<p class="text-success">Found ${filteredTools.length} tool(s).</p>`;
        displayTools(filteredTools);
    }
}
